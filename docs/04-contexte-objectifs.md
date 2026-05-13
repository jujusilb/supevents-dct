Contexte Technique

Le projet SupEvents consiste à transformer une gestion d'événements artisanale en un écosystème de microservices interopérables. 

L'enjeu majeur est de rompre avec les silos de données (fichiers Excel, formulaires papier) en centralisant la logique métier derrière des API REST sécurisées. 

L'expérience utilisateur doit être fluidifiée par une intégration SSO OIDC, réduisant le tunnel d'inscription à un simple échange de jetons d'authentification. 

Techniquement, la plateforme doit garantir une intégrité transactionnelle absolue (via une base relationnelle) pour éviter toute sur-réservation lors des pics de charge, tout en déléguant les processus lourds (notifications, paiements) à des systèmes tiers via un broker de messages. 

La conception doit privilégier une architecture stateless pour répondre aux exigences de haute disponibilité (99,5%) et permettre une montée en charge élastique sans interruption de service.



Objectifs techniques implicites

Objectif technique	Origine dans le CDC	Niveau de priorité

Scalabilité horizontale	"500 utilisateurs simultanés lors des pics" (ENF01)	Élevé

Conformité PCI-DSS par délégation	"Régler le montant via Stripe" / "Données ne transitent pas" (ENF03)	Critique

Conformité Accessibilité (WCAG 2.1 AA)	"Accessible à l'ensemble de la communauté" / "WCAG 2.1" (ENF04)	Moyen

Architecture Événementielle (Event-Driven)	"Notifications automatisées" / "Broker de messages obligatoire" (Contraintes)	Élevé

Haute Disponibilité (High Availability)	"Taux de disponibilité ≥ 99,5%" (ENF02)	Critique

Découplage des services	"Architecture orientée services" / "Pas de monolithe" (Contraintes)	Élevé

Auditabilité \& Observabilité	"Logs structurés JSON" / "Alertes seuils critiques" (ENF06)	Moyen

Intégrité des données financières	"Base de données relationnelle obligatoire pour transactions" (Contraintes)	Critique

