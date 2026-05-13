Terme|Définition|Première occurrence

SSO|Single Sign-On — Système permettant aux utilisateurs de Sup de Vinci d'utiliser leurs identifiants d'école pour accéder à la plateforme.|§2

OIDC|OpenID Connect — Protocole utilisé par le SSO pour transmettre l'identité de l'étudiant entre le serveur de l'école et SupEvents.|§2

API REST|"Interface permettant aux services de SupEvents (billetterie, catalogue) de communiquer entre eux via le protocole Web standard."|§5

RBAC,"Role-Based Access Control — Gestion des droits définissant les actions selon le profil (Étudiant, Organisateur, Administrateur)."|§3 (EF12)

RGPD|Règlement Général sur la Protection des Données — Obligation légale de protéger et permettre la suppression des données personnelles des étudiants.|§4 (ENF03)

PCI-DSS,Norme de sécurité bancaire. Sa délégation à Stripe garantit que SupEvents ne manipule jamais directement de numéros de carte.|§4 (ENF03)

Early-bird|"Tarif préférentiel ""premier arrivé, premier servi"" pour encourager les inscriptions rapides aux événements."|§3 (EF04)

CRUD|"Create, Read, Update, Delete — Les quatre actions de base (Créer, Voir, Modifier, Supprimer) possibles sur un événement."|§3 (EF01)

SLA|"Service Level Agreement — Engagement sur la disponibilité de 99,5%, assurant que le service est fonctionnel pour la communauté."|§4 (ENF02)

Webhook|Signal envoyé par Stripe ou SendGrid pour confirmer automatiquement à SupEvents qu'un paiement est validé ou qu'un mail est parti.|§5

Broker de messages|Intermédiaire technique gérant les tâches lourdes en différé (comme l'envoi des billets) pour ne pas bloquer l'utilisateur.|§5

P95|95e percentile — Mesure garantissant que 95% des étudiants bénéficient d'un temps de réponse ultra-rapide (moins de 500ms).|§4 (ENF01)

WCAG|"Web Content Accessibility Guidelines — Normes rendant la plateforme utilisable par tous les étudiants, incluant ceux avec un handicap."|§4 (ENF04)

i18n|Internationalisation — Architecture permettant l'affichage de l'interface en français et en anglais de manière fluide.|§4 (ENF05)

CI/CD|Continuous Integration / Deployment — Automatisation des tests et de la mise en ligne pour assurer la fiabilité des mises à jour.|§4 (ENF07)

CSV|Comma-Separated Values — Format d'exportation permettant aux organisateurs de récupérer la liste des inscrits sous Excel.|§3 (EF11)

