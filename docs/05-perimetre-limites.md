Dans le périmètre 	| 	Hors périmètre

Gestion des événements	|	Application mobile native

Catalogue public	|	Streaming \& Vidéo

Billetterie multitarif	|	Publicité \& Sponsoring

Paiement sécurisé	|	Synchronisation Calendrier

Émission de billets	|

Tableau de bord		|

Export de données	|

Authentification SSO	|

Administration		|

Conformité \& Standard	|







Limite technique	Justification

Migration de données existantes : Hors périmètreLe 	système est conçu "from scratch". Aucune base de données héritée (Legacy) n'a été identifiée ; l'initialisation se fera par saisie manuelle.

Support des navigateurs obsolètes (ex: IE11) : Hors périmètre	La cible étant étudiante, l'utilisation de navigateurs modernes (Evergreen browsers) est supposée. Cela permet l'usage de standards CSS/JS récents sans polyfills lourds.

Stockage direct des fichiers volumineux : Hors périmètre	Les visuels des événements ne seront pas stockés en base de données ni sur le serveur applicatif local, mais via un service de stockage d'objets (type S3) ou un CDN pour garantir la scalabilité.

Mode hors-ligne (PWA/Offline) : Hors périmètre		La plateforme nécessite une connectivité active pour garantir l'intégrité des stocks de places en temps réel et la validation des paiements Stripe.

Traitement d'images côté serveur : Hors périmètre	Le redimensionnement ou le recadrage complexe des visuels organisateurs ne sera pas géré par l'API ; les utilisateurs devront uploader des fichiers aux dimensions recommandées.

