.. _administration:

##############
Administration
##############

Libellés
########

.. _administration_collectivite:

=================
Les collectivités
=================

(:menuselection:`Administration --> Collectivité`)

Édition des collectivités présentes dans l'application.

.. _parametrage_parametre:

==============
Les paramètres
==============

(:menuselection:`Administration --> Paramètre`)

Édition des paramètres disponibles dans l'application.

Une convention de nommage existe. Il faut préfixer par :

* **ged\_** les paramètres spécifiques à la GED ;
* **erp\_** les paramètres spécifiques à ERP ;
* **option\_** les paramètres spécifiques aux options ;
* **id\_** les paramètres contenant un numéro d'identifiant ;
* **sig\_** les paramètres spécifiques au systeme d'information géographique ;
* **param_courriel_de_notification_commune\_** les paramètres spécifiques aux notifications par courriel aux communes ;
* **rapport_instruction\_** les paramètres spécifiques au rapport d'instruction.

Utilisation des options :

* **option_sig** : la valeur par défaut est *aucun*. Les valeurs possibles sont
  *sig_externe*, *sig_interne* ou *aucun*.
* **option_referentiel_arrete** : la valeur par défaut est *false*. Les valeurs 
  possibles sont *true* ou *false*.
* **option_localisation** : la valeur par défaut est *false*. Les valeurs possibles 
  sont *true* ou *false*.
* **option_erp** : la valeur par défaut est *false*. Les valeurs possibles sont 
  *true* ou *false*.
* **option_contrainte_di** : la valeur par défaut est *aucun*, exemple 
  d'utilisation "liste_groupe=g1,g2...;liste_ssgroupe=sg1,sg2...;service_consulte=t".
  Toutes les options sont optionnelles.
  Les options liste_groupe et liste_ssgroupe peuvent contenir une valeur unique 
  ou plusieurs valeurs séparées par une virgule, sans espace.
  La dernière option service_consulte permet d'ajouter une condition sur le champ
  du même nom. Il peut prendre t (Oui) ou f (Non) comme valeur.
* **option_afficher_division** : la valeur par défaut est *false*. Les valeurs 
  possibles sont *true* ou *false*.
* **option_arrondissement** : la valeur par défaut est *false*. Les valeurs 
  possibles sont *true* ou *false*.
  Cette option indique si la commune est divisée en arrondissements.
* **option_instructeur_division_numero_dossier** : la valeur par défaut est *false*. Les valeurs 
  possibles sont *true* ou *false*.
  Cette option indique si le numéro de dossier tient compte de la vision de l'instructeur auquel il est affecté.
* **option_portail_acces_citoyen** : permet d'activer ou de désactiver l'accès au :ref:`portail citoyen <portail_citoyen>`.
* **option_notification_piece_numerisee** : permet d'activer ou de désactiver l'ajout de :ref:`message de notification <instruction_dossier_message>`.
* **option_date_depot_demande_defaut** : permet d'afficher (*true*) ou non (*false*) la date du jour dans le champ de la date de dépôt lors de l'ajout d'une demande. La valeur par défaut est *true*.

.. note::

  La suppression d'une option entraîne la désactivation des fonctionnalités liées 
  à l'option.

Utilisation des paramètres de notification :

* **param_courriel_de_notification_commune** : paramètre commune listant les adresses mails de notification (une par ligne).
* **param_courriel_de_notification_commune_objet_depuis_instruction** : paramètre communauté spécifiant l'objet du courriel.
* **param_courriel_de_notification_commune_modele_depuis_instruction** : paramètre communauté (écrasable par la commune) spécifiant le modèle du corps du courriel.

.. _parametrage_parametre_mails_services_consultes:

Configuration des mails envoyés automatiquement aux services consultés :

* **services_consultes_lien_interne** : contient un lien d'accès en interne à openADS qui sera affiché dans le mail.
* **services_consultes_lien_externe** : contient un lien d'accès en externe à openADS qui sera affiché dans le mail.

.. note::

  Il est possible de renseigner des variables de remplacement dans l'objet et le corps du courriel :

  * **<DOSSIER_INSTRUCTION>** pour le numéro du dossier (objet et corps) ;
  * **<ID_INSTRUCTION>** pour l'identifiant unique de l'événement d'instruction (corps uniquement) ;
  * **<URL_INSTRUCTION>** pour le lien direct vers l'événement d'instruction (corps uniquement).
  
  Dans certains cas de figure, l'adresse **<URL_INSTRUCTION>** ne fonctionne pas. Si vous ne souhaitez pas faire appel à la génération automatique du lien, il faut écrire manuellement :

  **<a href="** *[LIEN]* **">** *[LIEN]* **</a>**

  en remplaçant *[LIEN]* par :

  *[SITE_WEB]* **/spg/direct_link.php?obj=dossier_instruction&action=3&direct_field=dossier&direct_form=instruction&direct_action=3&direct_idx=<ID_INSTRUCTION>**

  où *[SITE_WEB]* est l'adresse de la racine du logiciel (par exemple https://openads.maville.fr).


Gestion Des Utilisateurs
########################

.. _administration_profil:

===========
Les profils
===========

(:menuselection:`Administration --> Gestion Des Utilisateurs --> Profil`)

Édition des profils présents dans l'application.

.. _administration_droit:

==========
Les droits
==========

(:menuselection:`Administration --> Gestion Des Utilisateurs --> Droit`)

Édition des droits présents dans l'application.

.. _administration_utilisateur:

================
Les utilisateurs
================

(:menuselection:`Administration --> Gestion Des Utilisateurs --> Utilisateur`)

Édition des utilisateurs présents dans l'application.

.. _administration_annuaire:

==========
L'annuaire
==========

(:menuselection:`Administration --> Gestion Des Utilisateurs --> Annuaire`)

Gestion des utilisateurs grâce à un LDAP.

Tableaux de Bord
################


.. _administration_widget:

===========
Les widgets
===========

(:menuselection:`Administration --> Tableaux De Bord --> Widget`)

Un widget, contraction de window (fenêtre) et gadget, est un composant du
tableau de bord proposant des informations.

Son paramètrage nécessite la saisie de quatre champs :

* **libellé** : le titre du widget
* **type** : *file* lorsqu'il s'agit d'un script ou *web* lorsqu'il s'agit d'un
  appel à un web service
* **script** ou **lien** selon respectivement le type *file* ou *web* : nom du
  script ou URL du web service
* **arguments** ou **texte** selon respectivement le type *file* ou *web* :
  paramètres du script ou texte du widget (iframe, JavaScript, AJAX ...)

Seuls les widgets de type *file* sont utilisés dans openADS.

Les arguments sont déclarés ainsi :

::

  argument1=valeur1
  argument2=valeur2

Les scripts disponibles sont les suivants :

.. _administration_widget_consultation_retours:

consultation_retours
====================

Ce widget permet d'afficher le nombre de retours de consultation marqués comme 'non lu' pour les dossiers de l'utilisateur correspondant au filtre paramétrable. Un lien *Voir +* permet d'accéder au listing complet. Les informations fonctionnelles sont disponibles :ref:`ici<widget_consultation_retours>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *division* et *instructeur*


.. _administration_widget_dossiers_limites:

dossiers_limites
================

Ce widget permet d'afficher les dossiers d'instruction tacites dont la date limite est dans moins de X jours. Seuls les 10 premiers résultats sont affichés. Un lien *Voir +* permet d'accéder au listing complet. Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossiers_limites>`.

Trois arguments facultatifs sont paramétrables :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *division* et *instructeur*
* **nombre_de_jours** [par défaut *15*] - délai en jours avant la date limite à partir de laquelle on souhaite voir apparaître les dossiers
* **codes_datd** [par défaut tous les types sont affichés] - liste des types de dossiers à afficher séparés par un point-virgule. exemple : *PCI;PCA;DPS;CUa;CUb*


.. _administration_widget_messages_retours:

messages_retours
================

Ce widget permet d'afficher le nombre de messages en attente de lecture pour les dossiers de l'utilisateur correspondant au filtre paramétrable. Un lien *Voir +* permet d'accéder au listing complet. Les informations fonctionnelles sont disponibles :ref:`ici<widget_messages_retours>`.

Deux arguments facultatifs sont paramétrables :

* **contexte** [par défaut *standard*] - les contextes disponibles sont *standard* et *contentieux*
* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *division* et *instructeur*


.. _administration_widget_dossiers_evenement_incomplet_majoration:

dossiers_evenement_incomplet_majoration
=======================================

Ce widget présente les dossiers les plus récents (10 max.) sur lesquels ont été appliqué un événement de majoration ou d'incomplétude avec une date d'envoi de lettre RAR renseignée pour cet événement, et dont la date de retour RAR de l'événement n'a pas été complétée. Un lien "Voir tous les dossiers évènement incomplet ou majoration sans RAR" permet d'accéder au listing complet. Les informations fonctionnelles sont disponibles  :ref:`ici<widget_dossiers_evenement_incomplet_majoration>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *division* et *instructeur*


.. _administration_widget_nouvelle_demande_nouveau_dossier:

nouvelle_demande_nouveau_dossier
================================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_nouvelle_demande_nouveau_dossier>`.

Un argument facultatif est paramétrable :

* **contexte** [par défaut *standard*] - les contextes disponibles sont *standard* et *contentieux*.


.. _administration_widget_dossier_contentieux_recours:

dossier_contentieux_recours
===========================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_recours>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun* et *instructeur*.


.. _administration_widget_dossier_contentieux_infraction:

dossier_contentieux_infraction
==============================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_infraction>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun* et *instructeur*.


.. _administration_widget_dossier_contentieux_contradictoire:

dossier_contentieux_contradictoire
==================================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_contradictoire>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_widget_dossier_contentieux_ait:

dossier_contentieux_ait
=======================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_ait>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_widget_dossier_contentieux_audience:

dossier_contentieux_audience
============================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_audience>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_widget_dossier_contentieux_clotures:

dossier_contentieux_clotures
============================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_clotures>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_widget_dossier_contentieux_inaffectes:

dossier_contentieux_inaffectes
==============================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_inaffectes>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *aucun*] - les filtres disponibles sont *aucun* et *division*.


.. _administration_widget_dossier_contentieux_alerte_visite:

dossier_contentieux_alerte_visite
=================================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_alerte_visite>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_widget_dossier_contentieux_alerte_parquet:

dossier_contentieux_alerte_parquet
==================================

Les informations fonctionnelles sont disponibles :ref:`ici<widget_dossier_contentieux_alerte_parquet>`.

Un argument facultatif est paramétrable :

* **filtre** [par défaut *instructeur*] - les filtres disponibles sont *aucun*, *instructeur* et *division*.


.. _administration_composition:

===========
Composition
===========

(:menuselection:`Administration --> Tableaux De Bord --> Composition`)

Menu de composition du tableau de bord des utilisateurs.

Options Avancées
################


.. _administration_sousetat:

==============
Les sous-états
==============

(:menuselection:`Administration --> Options Avancées --> Sous États`)

Les sous-états des requêtes SQL.

.. _administration_omrequete:

===============
Les requêtes om
===============

(:menuselection:`Administration --> Options Avancées --> Om Requête`)

Les requêtes SQL des éditions.

.. _administration_import:

===========
Les imports
===========

(:menuselection:`Administration --> Options Avancées --> Import`)

Import des données au format CSV.

(:menuselection:`Administration --> Options Avancées --> Import spécifique`)

Import spécifique
=================

Ce menu permet d'accéder au module d'import des données au format ADS 2007.

Depuis le formulaire :

- importer le fichier csv
- choisir le séparateur (, ou ;)
- valider le formulaire d'import

.. NOTE:: L'encodage du fichier csv à importer doit être ISO-8859-15.
          
          Seuls les séparateurs , ou ; sont admis.
          
          Les références cadastrales doivent être séparées par une virgule.
  
Une fois le chargement terminé un récapitulatif des traitements effectués est affiché, dans celui-ci un fichier de rejet est disponible.

.. NOTE:: Si dans un dossier une date de decision est définie mais qu'il n'a pas de nature de decision alors le dossier est implicitement accordé.
.. NOTE:: Le suffixe "P0" est ajouté à la fin de chaque numéro de dossier initial seulement si le suffixe est activé pour le type de dossier d'instruction importé.

Ce fichier de rejet contient toutes les lignes du csv importées qui sont en erreur. Les erreurs sont ajoutées en fin de ligne dans une nouvelle colonne.

Exemple d'erreurs typiques :

- Le code INSEE n'est pas paramétré : un code INSEE doit être défini pour chaque commune dans les paramètres.
- Dossiers non clôturés (pas de date d'accord/rejet/refus et de date de décision).
- Mauvais format des références cadastrales.
- Dossier avec date de décision mais pas de nature de décision.

Après correction ce ficher de rejet peut être ré-importé.

Des dossiers importés peuvent être mis à jour hors d'openADS, lors du prochain import les données du dossiers et des données techniques seront mises à jour. Attention, les demandeurs ne sont pas mis à jour.

Description des colonnes du CSV :

+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Colonne | Nom de la colonne                   | Type    | Obligatoire | Description                                                                                                                                                                                    | Choix possibles                                                                                                                                                                 |
+=========+=====================================+=========+=============+================================================================================================================================================================================================+=================================================================================================================================================================================+
| 1       | Type                                | texte   | Oui         | Code des types de dossiers d'autorisations                                                                                                                                                     | AZ, AT, AC, ST, CH, CX, CS, CA, DF, DT, MH, DP, CO, FA, IN, LT, NR, TP, PA, PC, PI, PD, RE, RD, SC, CI, CUb, CUa, DPS                                                           |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2       | Numéro                              | texte   | Oui         | Identifiant du dossier                                                                                                                                                                         |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3       | Initial                             | texte   | Non         | Identifiant du dossier initial                                                                                                                                                                 |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4       | INSEE                               | entier  | Oui         | Code INSEE de la commune sur 5 caractères                                                                                                                                                      | Le code INSEE doit être paramétré pour chaque commune (Administration → Paramètres)                                                                                             |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5       | Commune                             | texte   | Non         | Nom de la commune                                                                                                                                                                              | Les commune doivent être créées (Administration → Collectivité)                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6       | Autonome                            | Oui/Non | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7       | Projet                              | texte   | Non         | Description du projet d'urbanisme /!\ Attention : quelle que soit la nature de la DP/DPS (construction, démolition ou aménagement), ce texte se mettra dans la description de la construction. |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8       | Destination                         | texte   | Non         | Affectation de la construction (choix multiples)                                                                                                                                               | Habitation, Hébergement hôtelier, Bureaux, Commerce, Artisanat, Industrie, Exploit. agricole ou forestière, Entrepôt, Service public ou d'intérêt général                       |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 9       | Nb logements                        | integer | Non         | Nombre de logements                                                                                                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 10      | Surface terrain                     | décimal | Non         | Surface du terrain                                                                                                                                                                             |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 11      | SHON existante                      | décimal | Non         | SHON existante                                                                                                                                                                                 |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 12      | SHON construite                     | décimal | Non         | SHON construite                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 13      | SHON transformation SHOB            | décimal | Non         | SHON transformation SHOB                                                                                                                                                                       |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 14      | SHON changement destination         | décimal | Non         | SHON changement destination                                                                                                                                                                    |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 15      | SHON démolie                        | décimal | Non         | SHON démolie                                                                                                                                                                                   |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 16      | SHON supprimée                      | décimal | Non         | SHON supprimée                                                                                                                                                                                 |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 17      | Architecte                          | Oui/Non | Non         | Soumis à architecte O/N                                                                                                                                                                        |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 18      | Demandeur                           | texte   | Oui         | Nom du demandeur                                                                                                                                                                               |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 19      | Opposition CNIL                     | Oui/Non | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 20      | Adresse demandeur                   | texte   | Non         | Adresse principale du demandeur                                                                                                                                                                | L'adresse du demandeur doit être de la forme : [adresse (90 caractères max)] [code postal (5 chiffre)] [commune (30 caractères max)]                                            |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 21      | Terrain                             | texte   | Non         | Adresse de la construction                                                                                                                                                                     | L'adresse du terrain doit être de la forme : [adresse (90 caractères max)] [code postal (5 chiffre)] [commune (30 caractères max)]                                              |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 22      | Références cadastrales              | texte   | Non         | Références cadastrales (séparées par ",")                                                                                                                                                      | Format des références : 0 à 4 chiffres, 1 à 2 lettres (obligatoires), 1 à 4 chiffres (obligatoire). Chaque partie est séparée par un tiret. Exemple : 123-AA-0123, AB-0123, ... |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 23      | Lotissement                         | Oui/Non | Non         | Rattachement à un lotissement                                                                                                                                                                  |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 24      | AFU                                 | Oui/Non | Non         | Statut d'Association foncière urbaine                                                                                                                                                          |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 25      | Détail ZAC AFU                      | texte   | Oui         | Description opération d'aménagement de type AFU                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 26      | Autorité                            | texte   | Oui         | Code de l'autorité référente au dossier                                                                                                                                                        | COM, ETATMAIRE, ETAT                                                                                                                                                            |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 27      | Etat                                | texte   | Non         | État du dossier                                                                                                                                                                                | retire, annule, accepte_tacite, accepter, rejeter, Sursis_a_statuer, terminer, refuse_tacite, refuse                                                                            |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 28      | Centre instructeur                  | texte   | Non         | Centre instructeur                                                                                                                                                                             |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 29      | Instructeur                         | texte   | Non         | Nom de l'intructeur                                                                                                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 30      | Liquidateur                         | texte   | Non         | Nom du liquidateur                                                                                                                                                                             |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 31      | Complexité                          | texte   | Oui         | Niveau d'enjeu du dossier (Forte/Moyenne/Faible)                                                                                                                                               |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 32      | Dépôt en mairie                     | date    | Oui         | Date de dépôt en mairie                                                                                                                                                                        |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 33      | Réception DDE                       | date    | Non         | Date de réception par le service instructeur de la DDE                                                                                                                                         |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 34      | Complétude                          | date    | Non         | Date de réception des pièces complémentaires demandées                                                                                                                                         |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 35      | Notification majoration             | date    | Oui         | Date de notification de la majoration si dossier complet                                                                                                                                       |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 36      | DLI                                 | date    | Non         | Date limite d'instruction                                                                                                                                                                      |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 37      | Date envoi demande de pièces        | date    | Non         | Date envoi demande de pièces                                                                                                                                                                   |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 38      | Date notification demande de pièces | date    | Non         | Date notification demande de pièces                                                                                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 39      | Date envoi délai majoration         | date    | Non         | Date envoi délai majoration                                                                                                                                                                    |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 40      | Date notification délai majoration  | date    | Non         | Date notification délai majoration                                                                                                                                                             |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 41      | Service consulté                    | texte   | Non         | Services extérieurs consultés                                                                                                                                                                  |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 42      | Proposition service                 | texte   | Non         | Proposition du service consulté                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 43      | Date proposition service            | date    | Non         | Date de proposition du service consulté                                                                                                                                                        |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 44      | Date transmission proposition       | date    | Non         | Date de transmission de l'arrêté du service instructeur                                                                                                                                        |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 45      | Date instruction terminée           | date    | Non         | Date instruction terminée                                                                                                                                                                      |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 46      | Date accord tacite                  | date    | Non         | Date accord tacite                                                                                                                                                                             |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 47      | Date de rejet tacite                | date    | Non         | Date de rejet tacite                                                                                                                                                                           |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 48      | Date de refus tacite                | date    | Non         | Date de refus tacite                                                                                                                                                                           |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 49      | Date de décision                    | date    | Oui         | Date de décision                                                                                                                                                                               |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 50      | Date notification décision          | date    | Non         | Date notification décision                                                                                                                                                                     |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 51      | Nature décision                     | texte   | Oui         | Nature de la décision                                                                                                                                                                          | Defavorable, Favorable, Annulation, Refus tacite, Sursis a statuer, Accord Tacite, Favorable avec Reserves, Rejet tacite, Annulation par tribunal                               |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 52      | Récolement                          | texte   | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 53      | DOC                                 | date    | Non         | Date de déclaration d'ouverture de chantier                                                                                                                                                    |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 54      | DAACT                               | date    | Non         | Date d'achèvement et de conformité des travaux                                                                                                                                                 |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 55      | Type Evolution                      | texte   | Non         | Type d'évolution de l'autorisation (Prorogation, Retrait à l'initative du pétitionnaire, Transfert)                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 56      | Statut Evolution                    | texte   | Non         | État de l'évolution en cours (Évolution en cours, Décision notifiée au demandeur)                                                                                                              |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 57      | Type dernières taxes                | texte   | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 58      | Statut dernières taxes              | texte   | Non         | Non taxable, Taxe initiale, Dégrèvement, Exonération, Procès verbal                                                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 59      | Type dernière RAP                   | texte   | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 60      | Statut dernière RAP                 | texte   | Non         | Non taxable, Taxe initiale, Dégrèvement, Exonération, Procès verbal                                                                                                                            |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 61      | EPCI                                | texte   | Non         |                                                                                                                                                                                                |                                                                                                                                                                                 |
+---------+-------------------------------------+---------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


.. _administration_generateur:

=============
Le générateur
=============

(:menuselection:`Administration --> Options Avancées --> Générateur`)

Le générateur de fichiers de l'application.
