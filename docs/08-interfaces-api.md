08 — Interfaces \& Contrats d'API

B.1 — Tableau synoptique des endpoints REST

L'API suit les principes RESTful avec un versioning systématique. Tous les échanges se font en JSON, sauf pour les exports de données (CSV).



Méthode	Chemin	Description	Auth	Codes retour	Dépendances aval

GET	/api/v1/auth/callback	Callback pour le SSO OIDCPublic200, 401, 500AuthService, SSO École

POST	/api/v1/auth/refreshRenouvellement du JWTPublic200, 400, 401AuthService, RedisPOST/api/v1/auth/logoutRévocation de la sessionJWT204, 401AuthService, Redis

GET	/api/v1/eventsListe/Recherche d'événementsPublic200, 400EventService, PostgreSQLGET/api/v1/events/{id}Détail d'un événementPublic200, 404EventService

POST	/api/v1/ticketsRéservation d'une place (Pending)JWT201, 400, 403, 409TicketService, EventService

GET	/api/v1/tickets/meListe des tickets de l'étudiantJWT200, 401TicketService

POST	/api/v1/paymentsInitiation de session de paiementJWT201, 400, 402PaymentService, Stripe

POST	/api/v1/payments/webhookConfirmation de paiementHMAC200, 400PaymentService, Stripe

GET	/api/v1/orga/dashboardKPI et stats de remplissageJWT (Orga)200, 403EventService, TicketService

GET	/api/v1/orga/events/{id}/csv	Export des participants (CSV)JWT (Orga)200, 403, 404TicketService

PATCH	/api/v1/admin/organizers/{id}	Valider/Révoker un organisateurJWT (Admin)200, 401, 403UserService



B.2 — Documentation des événements asynchrones

1\. Événement : ticket.confirmed

Cet événement est publié dès qu'une inscription est considérée comme définitive (paiement Stripe validé ou ticket gratuit validé).

Fiche descriptive :

Champ	Valeur

Nom	ticket.confirmed

Producteur	TicketService (après confirmation du paiement ou validation jauge)

Topic / Exchange	tickets.events.v1

Consommateurs	NotificationService (envoi email SendGrid), PDFService (génération billet)

Garantie	at-least-once (via RabbitMQ Acknowledgments)

Stratégie de retry	Exponentielle (10s, 60s, 5min), max 5 tentatives, puis Dead Letter Queue (DLQ)



Schéma JSON :

{

&#x20; "$schema": "http://json-schema.org/draft-07/schema#",

&#x20; "title": "TicketConfirmedEvent",

&#x20; "type": "object",

&#x20; "required": \["event\_id", "ticket\_id", "user\_id", "confirmed\_at"],

&#x20; "properties": {

&#x20;   "event\_id": { "type": "string", "format": "uuid" },

&#x20;   "ticket\_id": { "type": "string", "format": "uuid" },

&#x20;   "user\_id": { "type": "string", "format": "uuid" },

&#x20;   "user\_email": { "type": "string", "format": "email" },

&#x20;   "event\_title": { "type": "string" },

&#x20;   "confirmed\_at": { "type": "string", "format": "date-time" },

&#x20;   "qr\_code\_data": { "type": "string" }

&#x20; }

}



2\. Événement : event.cancelled

Publié lorsqu'un organisateur décide d'annuler un événement. Il déclenche les processus de remboursement et d'information.



Fiche descriptive :

Champ	Valeur

Nome	vent.cancelled

Producteur	EventService (action manuelle de l'organisateur ou administrateur)

Topic / Exchange	events.management.v1

Consommateurs	PaymentService (initie remboursements), NotificationService (alerte inscrits)

Garantie	at-least-once

Stratégie de retry	Linéaire (30s), max 3 tentatives, puis alerte critique (Monitoring)



Schéma JSON :

{

&#x20; "$schema": "http://json-schema.org/draft-07/schema#",

&#x20; "title": "EventCancelledEvent",

&#x20; "type": "object",

&#x20; "required": \["event\_id", "cancelled\_at", "reason"],

&#x20; "properties": {

&#x20;   "event\_id": { "type": "string", "format": "uuid" },

&#x20;   "organizer\_id": { "type": "string", "format": "uuid" },

&#x20;   "cancelled\_at": { "type": "string", "format": "date-time" },

&#x20;   "reason": { "type": "string", "minLength": 10 },

&#x20;   "impacted\_tickets\_count": { "type": "integer", "minimum": 0 }

&#x20; }

}

Justification de la sémantiqueNotez l'usage du participe passé (confirmed, cancelled). Cela signifie que le changement d'état est déjà acté en base de données PostgreSQL. Le système ne demande pas la permission d'annuler, il informe le reste de l'écosystème que l'annulation est une vérité métier consommable.

