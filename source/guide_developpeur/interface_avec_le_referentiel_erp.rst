.. _interface_avec_le_referentiel_erp:

#################################
Interface avec le référentiel ERP
#################################


Interface avec le logiciel openARIA : http://www.openmairie.org/catalogue/openaria


Configuration
#############

*Activation de l'option dans l'interface* :

L'option suivante permet d'activer l'option des messages sortants vers le référentiel ERP.

om_parametre

- **option_referentiel_erp** -> true


*Configuration des paramètres des déclencheurs* :

 - **erp__dossier__nature__at** : correspond au code du type de DA détaillé qui représente une autorisation de travaux par exemple "AT" (utilisé par les échanges [109][112][110][108][101]).
 - **erp__dossier__nature__pc** : correspond au code du type de DA détaillé qui représente un permis de construire par exemple "PC" (utilisé par les échanges [104][106][107][102][103][105][111]).
 - **erp__demandes__depot_piece__at** : correspond à une liste d'identifiants de types de demandes qui représentent le dépôt de pièce sur une AT par un pétitionnaire au guichet par exemple "12;16;18" (utilisé par l'échange [112])
 - **erp__demandes__retrait__at** : correspond à une liste d'identifiants de types de demandes qui représentent la demande de retrait du dossier AT par un pétitionnaire au guichet par exemple "12" (utilisé par l'échange [109])
 - **erp__demandes__ouverture__at** : correspond à une liste d'identifiants de types de demandes qui représentent la demande de d'ouverture d'un ERP sur un dossier AT par un pétitionnaire au guichet par exemple "16;18" (utilisé par l'échange [110])
 - **erp__demandes__ouverture__pc** : correspond à une liste d'identifiants de types de demandes qui représentent la demande de d'ouverture d'un ERP sur un dossier PC par un pétitionnaire au guichet par exemple "16;18" (utilisé par l'échange [107])
 - **erp__services__avis__pc** : correspond à une liste d'identifiants de services qui représentent les services ERP qui sont notifiés lors d'une consultation pour avis par exemple "16;18" (utilisé par l'échange [104])
 - **erp__services__conformite__pc** :  correspond à une liste d'identifiants de services qui représentent les services ERP qui sont notifiés lors d'une consultation pour confirmité par exemple "16" (utilisé par l'échange [106])
 - **erp__evenements__decision__pc** : correspond à une liste d'identifiants d'événements sur lesquels les services ERP doivent être notifiés sur un dossier PC par exemple "16;18" (utilisé par les échanges [105][111])
 - **id_evenement_cloture_at** : correspond à l'identifiant de l'événement à déclencher lors de la réception d'un message de clôture d'une AT (utilisé par l'échange [211]). Pour ne pas être bloquant pour l'autre application, l'événement de workflow doit pouvoir être ajouté plusieurs fois sur le même dossier.
 - **id_evenement_completude_at** : correspond à l'identifiant de l'événement à déclencher lors de la réception d'un message de complétude/incomplétude d'une AT (utilisé par l'échange [210]). Pour ne pas être bloquant pour l'autre application, l'événement de workflow doit pouvoir être ajouté plusieurs fois sur le même dossier.
 
.. _configuration_echanges_sortants_referentiel_erp:

*Configuration des URLs des échanges sortants vers le référentiel ERP* :


``dyn/services.inc.php``

.. code-block:: php

    //
    $ERP_URL_MESSAGES = 'http://openaria/services/rest_entry.php/messages/';



Les échanges
############


.. _echange_ads_erp_101:

====================================================================
[101](Échange ADS → ERP) Dossier AT Information de qualification ADS
====================================================================

L'objectif principal de cet échange est de permettre à l'instructeur ADS de transmettre facilement les informations d'autorité compétente et de contraintes PLU (de compétence URBA) du dossier AT aux services ERP.


*Identifiant* : ADS_ERP__AT__INFORMATION_DE_QUALIFICATION_ADS


*Cas d'utilisation* :

• Suite à un échange [108], l'instructeur ADS a la possibilité de qualifier un dossier AT en lui renseignant les informations d'autorité compétente et de contraintes PLU qui sont des informations de compétence URBA. Lorsqu'il estime avoir qualifié un dossier, il le note comme qualifié dans le formulaire de modification du dossier et un message informe directement les services ERP des informations renseignées.


*Déclencheur* :

• L'option ERP est activée 
• Le dossier est de type AT (paramètre 'erp__dossier_nature__at')
• Le dossier est marqué comme « connecté au référentiel ERP »
• Le formulaire de modification du dossier est validé avec le marqueur « à qualifier » à « NON » (dossier::triggermodifier())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **competence** : Compétence : texte : Qualification compétence : Mairie/Etat
  - **contraintes_plu** : Contraintes PLU : texte multilignes reprenant les contraintes PLU du dossier
  - **references_cadastrales** : 


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AT__INFORMATION_DE_QUALIFICATION_ADS",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0",
        "contenu" : {
            "competence" : "",
            "contraintes_plu" : "",
            "references_cadastrales" : ""
        }
    }


.. _echange_ads_erp_102:

=====================================================================
[102](Échange ADS → ERP) Dossier PC/ERP Pré-demande de complétude ERP
=====================================================================

L'objectif principal de cet échange est de permettre à l'instructeur ADS de gagner du temps dans sa vérification de complétude et d'interroger rapidement les services ERP sur la complétude du dossier.


*Identifiant* : ADS_ERP__PC__PRE_DEMANDE_DE_COMPLETUDE_ERP


*Cas d'utilisation* :

• Lors de la qualification d'un dossier ADS par un instructeur, celui-ci peut qualifier le dossier comme étant aussi ERP. Un message est alors transmis au Service ERP pour les pré-notifier avant la consultation officielle du service pour gagner du temps sur la complétude du dossier. Cet échange n'est pas une consultation avec demande d'avis.


*Déclencheur* :

• L'option ERP est activée
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le formulaire de modification du dossier est validé avec le marqueur « à qualifier » à « NON » ET le marqueur « ERP » à « OUI » (dossier::triggermodifier())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Marquage du dossier PC : Le marqueur « connecté avec le référentiel ERP » sur le dossier est positionnée à « OUI » afin de pouvoir identifier ce dossier à l'avenir.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__PRE_DEMANDE_DE_COMPLETUDE_ERP",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0"
    }


.. _echange_ads_erp_103:

========================================================================
[103](Échange ADS → ERP) Dossier PC/ERP Pré-demande de qualification ERP
========================================================================

L'objectif principal de cet échange est de permettre à l'instructeur ADS de gagner du temps dans sa qualification de dossier et d'interroger rapidement les services ERP sur le caractère ERP du dossier.


*Identifiant* : ADS_ERP__PC__PRE_DEMANDE_DE_QUALIFICATION_ERP


*Cas d'utilisation* :

• Lors de la qualification d'un dossier PC par un instructeur ADS, celui-ci peut qualifier le dossier PC comme étant aussi ERP. Un message est alors transmis au Service ERP pour qualification du dossier. Cet échange n'est pas une consultation avec demande d'avis.


*Déclencheur* :

• L'option ERP est activée
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le formulaire de modification du dossier est validé avec le marqueur « à qualifier » à « NON » ET le marqueur « ERP » à « OUI » (dossier::triggermodifier())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Marquage du dossier PC : Le marqueur « connecté avec le référentiel ERP » sur le dossier est positionnée à « OUI » afin de pouvoir identifier ce dossier à l'avenir.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__PRE_DEMANDE_DE_QUALIFICATION_ERP",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0"
    }


.. _echange_ads_erp_104:

=========================================================================
[104](Échange ADS → ERP) Dossier PC/ERP Consultation officielle pour avis
=========================================================================

L'objectif principal de cet échange est de permettre à l'instructeur ADS d'émettre une consultation officielle pour avis des services ERP.


*Identifiant* : ADS_ERP__PC__CONSULTATION_OFFICIELLE_POUR_AVIS


*Cas d'utilisation* :

• Dans le cadre de l'instruction ADS d'un dossier PC, l'instructeur consulte un service ERP pour avis. Une notification est transmise à penARIA, pour prise en charge par les services ERP.


*Déclencheur* :

• L'option ERP est activée
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le formulaire d'ajout de consultation est validé avec un service correspondant à un des services ERP pour avis (paramètre erp__services__avis') (consultation::triggerajouter())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Marquage du dossier PC : Le marqueur « connecté avec le référentiel ERP » sur le dossier est positionnée à « OUI » afin de pouvoir identifier ce dossier à l'avenir.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **consultation** : Identifiant de la consultation
  - **service_abrege** : Code du service consulté
  - **service_libelle** : Libellé du service consulté
  - **date_envoi** : Date d'envoi de la consultation
  - **date_limite** : Date limite de réponse


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__CONSULTATION_OFFICIELLE_POUR_AVIS",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0",
        "contenu" : {
            "consultation" : 2,
            "date_envoi" : "31/12/2015",
            "service_abrege" : "ACC",
            "service_libelle" : "Service Accessibilité",
            "date_limite" : "31/01/2016",
        }
    }


.. _echange_ads_erp_105:

===================================================================
[105](Échange ADS → ERP) Dossier PC/ERP Information de décision ADS
===================================================================

L'objectif principal de cet échange est de permettre d'informer les services ERP de certaines étapes importantes de la vie du dossier : arrêté effectué, retrait du dossier par le pétitionnaire, ...


*Identifiant* : ADS_ERP__PC__INFORMATION_DE_DECISION_ADS


*Cas d'utilisation* :

• Ce message est un message envoyé par ADS à ERP suite à un événement dans le cadre du suivi d'instruction du dossier : arrêté effectué, retrait du dossier, décision de conformité, ...


*Déclencheur* :

• L'option ERP est activée
• Le dossier est marqué comme « connecté au référentiel ERP »
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Ajout d'un événement d'instruction sur le dossier dont l'identifiant correspond aux événements dont les services ERP doivent être informé (paramètre 'erp__evenements_decision__pc') (instruction::triggerajouterapres())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **decision** : Décision : texte libre (Décision de l'arrêté)


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__INFORMATION_DE_DECISION_ADS",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0",
        "contenu" : {
            "decision" : ""
        }
    }


.. _echange_ads_erp_106:

===============================================================================
[106](Échange ADS → ERP) Dossier PC/ERP Consultation officielle pour conformité
===============================================================================

L'objectif principal de cet échange est de permettre à l'instructeur ADS de gagner du temps dans sa consultation officielle pour conformité des services ERP.


*Identifiant* : ADS_ERP__PC__CONSULTATION_OFFICIELLE_POUR_CONFORMITE


*Cas d'utilisation* :

• Message transmis lors de l'instruction du Dossier d'Instruction de DAACT destiné à analyser la conformité d'un Dossier d'Autorisation ADS


*Déclencheur* :

• L'option ERP est activée
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le formulaire d'ajout de consultation est validé avec un service correspondant à un des services ERP pour conformité (paramètre 'erp__services__conformite') (consultation::triggerajouter())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Marquage du dossier PC-DAACT : Le marqueur « connecté avec le référentiel ERP » sur le dossier créé est positionnée à « OUI » afin de pouvoir identifier ce dossier à l'avenir.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message "Consultation ERP pour conformité"
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **consultation** : Identifiant de la consultation
  - **service_abrege** : Code du service consulté
  - **service_libelle** : Libellé du service consulté
  - **date_envoi** : Date d'envoi de la consultation
  - **date_limite** : Date limite de réponse


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__CONSULTATION_OFFICIELLE_POUR_CONFORMITE",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0",
        "contenu" : {
            "consultation" : 2,
            "date_envoi" : "31/12/2015",
            "service_abrege" : "SC",
            "service_libelle" : "Service Conformité",
            "date_limite": "31/01/2016"
        }
    }


.. _echange_ads_erp_107:

=====================================================================
[107](Échange ADS → ERP) Dossier PC/ERP Demande de visite d'ouverture
=====================================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est d'informer les services ERP qu'une demande de visite d'ouverture a été déposée.


*Identifiant* : ADS_ERP__PC__DEMANDE_DE_VISITE_D_OUVERTURE_ERP


*Cas d'utilisation* :

• Message transmis lors d'un dépôt de Demande d'ouverture ERP lié à un PC au Guichet Unique.


*Déclencheur* :

• L'option ERP est activée
• Le formulaire d'ajout de demande est validé avec un type de demande correspondant à une demande de visite d'ouverture ERP (paramètre 'erp__demandes__ouverture__pc') (demande::triggerajouter())
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le dossier est marqué comme « connecté au référentiel ERP »


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__PC__DEMANDE_DE_VISITE_D_OUVERTURE_ERP",
        "date" : "31/12/2015 14:42",
        "emetteur" : "instr",
        "dossier_instruction" : "PC0130551600001P0"
    }


.. _echange_ads_erp_108:

=================================================
[108](Échange ADS → ERP) Dossier AT Dépôt initial
=================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est d'informer les services ERP qu'une demande d'autorisation de travaux a été déposée.


*Identifiant* : ADS_ERP__AT__DEPOT_INITIAL


*Cas d'utilisation* :

• Lors du dépôt d'un nouveau dossier de type AT au Guichet Unique par le pétitionnaire, les agents du guichet saisissent la demande et un message en informe directement les services ERP. Le dossier créé est également marqué comme « connecté avec le référentiel ERP ».


*Déclencheur* :

• L'option ERP est activée
• Validation du formulaire d'ajout d'une demande de nouveau dossier de type AT (paramètre 'erp__dossier_nature__at') (dossier::triggerajouter())


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Marquage du dossier AT : Le marqueur « connecté avec le référentiel ERP » sur le dossier créé est positionnée à « OUI » afin de pouvoir identifier ce dossier à l'avenir.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AT__DEPOT_INITIAL",
        "date" : "31/12/2015 14:42",
        "emetteur" : "guichet",
        "dossier_instruction" : "AT0130551600001P0"
    }


.. _echange_ads_erp_109:

============================================================
[109](Échange ADS → ERP) Dossier AT Retrait du pétitionnaire
============================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est d'informer les services ERP qu'une demande de retrait d'autorisation de travaux a été déposée.


*Identifiant* : ADS_ERP__AT__RETRAIT_DU_PETITIONNAIRE


*Cas d'utilisation* :

• Message transmis au logiciel ERP lors du dépôt d'une demande d'annulation au Guichet Unique, pour les dossiers ERP (DAT) ou marqués ERP (PC ERP)


*Déclencheur* :

• L'option ERP est activée
• Le formulaire d'ajout de demande est validé avec un type de demande correspondant à une demande de retrait (paramètre 'erp__demandes__retrait__at') (demande::triggerajouter())
• Le dossier est de type AT (paramètre 'erp__dossier_nature__at')
• Le dossier est marqué comme « connecté au référentiel ERP »


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AT__RETRAIT_DU_PETITIONNAIRE",
        "date" : "31/12/2015 14:42",
        "emetteur" : "guichet",
        "dossier_instruction" : "AT0130551600001P0"
    }


.. _echange_ads_erp_110:

=================================================================
[110](Échange ADS → ERP) Dossier AT Demande de visite d'ouverture
=================================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est d'informer les services ERP qu'une demande de visite d'ouverture a été déposée.


*Identifiant* : ADS_ERP__AT__DEMANDE_DE_VISITE_D_OUVERTURE_ERP


*Cas d'utilisation* :

• Le pétitionnaire dépose au guichet unique une demande de visite d'ouverture ERP sur une autorisation de travaux. Le guiche unique lui remet un récepissé et informe les services ERP.


*Déclencheur* :

• L'option ERP est activée
• Le formulaire d'ajout de demande est validé avec un type de demande correspondant à une demande de visite d'ouverture ERP (paramètre 'erp__demandes__ouverture__at') (demande::triggerajouter())
• Le dossier est de type AT (paramètre 'erp__dossier_nature__at')
• Le dossier est marqué comme « connecté au référentiel ERP »


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AT__DEMANDE_DE_VISITE_D_OUVERTURE_ERP",
        "date" : "31/12/2015 14:42",
        "emetteur" : "guichet",
        "dossier_instruction" : "AT0130551600001P0"
    }


.. _echange_ads_erp_111:

==========================================================================
[111](Échange ADS → ERP) Dossier PC/ERP Information de décision Conformité
==========================================================================

L'objectif principal de cet échange est de permettre d'informer les services ERP de certaines étapes importantes de la vie du dossier : arrêté effectué, retrait du dossier par le pétitionnaire, ...


*Identifiant* : ADS_ERP__PC__DECISION_DE_CONFORMITE_EFFECTUEE


L'échange [105] a été rendu plus générique et permet de réaliser l'objectif de cet échange. Celui-ci a donc été supprimé.


.. _echange_ads_erp_112:

=======================================================================
[112](Échange ADS → ERP) Dossier AT Dépôt de pièce par le pétitionnaire
=======================================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est d'informer les services ERP qu'un dépôt de pièces a été fait.


*Identifiant* : ADS_ERP__AT__DEPOT_DE_PIECE_PAR_LE_PETITIONNAIRE


*Cas d'utilisation* :

• Ce message (analogue au message [108]) complète les messages [210] et [211] en permettant aux agents du Guichet Unique de signaler l'arrivée d'une nouvelle pièce aux agents d'ERP. Si le Dossier d'instruction est ouvert, alors les pièces sont acceptées (si le dossier est « incomplet » les pièces sont classées « complémentaires », sinon les pièces sont classées « supplémentaires »). Dans les deux cas, openADS envoie automatiquement un message unique à openARIA signalant l'arrivée d'une pièce sur le dossier et son statut : pièce « complémentaire » ou « supplémentaire ».


*Déclencheur* :

• L'option ERP est activée
• Le formulaire d'ajout de demande est validé avec un type de demande correspondant à une demande de dépôt de pièces (paramètre 'erp__demandes__depot_piece__at') (demande::triggerajouter())
• Le dossier est de type AT (paramètre 'erp__dossier_nature__at')
• Le dossier est marqué comme « connecté au référentiel ERP »


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu du message* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **type_piece** : Si le Dossier d'instruction est ouvert, alors les pièces sont acceptées (si le dossier est « incomplet » les pièces sont classées « complémentaires », sinon les pièces sont classées « supplémentaires »). Dans les deux cas, openADS envoie automatiquement un message unique à openARIA signalant l'arrivée d'une pièce sur le dossier et son statut : pièce « complémentaire » ou « supplémentaire ».


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AT__DEPOT_DE_PIECE_PAR_LE_PETITIONNAIRE",
        "date" : "31/12/2015 14:42",
        "emetteur" : "admin",
        "dossier_instruction" : "AT0130551600001P0",
        "contenu": {
            "type_piece" : "complémentaire"
        }
    }


.. _echange_ads_erp_113:

=============================================================
[113](Échange ADS → ERP) Ajout d'une nouvelle pièce numérisée
=============================================================

L'objectif principal de cet échange est de permettre aux services ERP d'être informé de la numérisation d'une pièce sur un dossier sur lequel ils sont impliqués.


*Identifiant* : ADS_ERP__AJOUT_D_UNE_NOUVELLE_PIECE_NUMERISEE


*Cas d'utilisation* :

• Message transmis lors de l'ajout d'une nouvelle pièce sur un dossier de type AT ou un dossier de type PC qui concerne un ERP.


*Déclencheur* :

• L'option ERP est activée
• Le dossier est marqué comme « connecté au référentiel ERP »
• Ajout d'une nouvelle pièce.


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.


*Contenu de l'échange* :

- **type** : Type de message
- **date** :  Date/heure d'envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l'utilisateur à l'origine du message)
- **dossier_instruction** : Identifiant du dossier d'instruction
- **contenu** :

  - **date_creation** : Date de création
  - **nom_fichier** : Nom du fichier : texte
  - **type** : Type de document : texte
  - **categorie** : Catégorie du type de document


*Exemple* :

.. sourcecode:: http
      
    POST /openaria/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type" : "ADS_ERP__AJOUT_D_UNE_NOUVELLE_PIECE_NUMERISEE",
        "date" : "31/12/2015 14:42",
        "emetteur" : "admin",
        "dossier_instruction" : "AT0130551600001P0",
        "contenu": {
            "date_creation" : "31/12/2015",
            "nom_fichier" : "DGIMPC.pdf",
            "type" : "Imprimé de demande de permis de construire",
            "categorie" : "Définition Générale"
        }
    }



.. _echange_ads_erp_114:

========================================================================
[114](Échange ADS → ERP) Dossier PC Notification de dossier à enjeu ADS
========================================================================

L'objectif principal de cet échange est de permettre aux services ADS de partager le caractère 'à enjeu' du dossier pour en informer le service ERP.


*Identifiant* : ADS_ERP__PC__ENJEU_ADS


*Cas d'utilisation* :

• Un instructeur peut qualifier le dossier comme dossier à enjeu. Dans ce cas, un message « Dossier à enjeu ADS » est envoyé vers l'application ERP afin de mettre à jour le dossier de coordination. La mise à jour est effectuée automatiquement et un message est présenté au service ERP qui est chargé de mettre à jour le dossier. 

*Déclencheur* :

• L'option ERP est activée
• Le dossier est de type PC (paramètre 'erp__dossier_nature__pc')
• Le dossier est marqué comme « connecté au référentiel ERP »
• Le formulaire de modification de dossier dossier d'instruction avec « enjeu urbanisme » qui change de statut OU le marqueur « à qualifier » qui passe de « OUI » à « NON » ET « enjeu urbanisme » à « OUI » (dossier::triggermodifierapres())

*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Envoi de la requête à destination de la ressource 'messages' d'openARIA. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_erp>`.



*Contenu de l'échange* :

- **type** : Type de message
- **date** : Date/heure d’envoi du message
- **emetteur** : Émetteur du message (Nom/Prénom/Login de l’utilisateur à l’origine du message)
- **dossier_instruction** : Identifiant du dossier d’instruction
- **contenu** :

  - **Dossier à enjeu ADS** : Oui / Non


*Exemple* :

.. sourcecode:: http
      
    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ADS_ERP__PC__ENJEU_ADS",
        "date": "10/01/2017 12:52",
        "emetteur": "admin",
        "dossier_instruction": "PC0130551601234P0",
        "contenu": {
             "Dossier à enjeu ADS": "oui"
        }
    }



.. _echange_erp_ads_201:

=========================================================================================
[201](Échange ERP → ADS) Mise à jour du numéro de l'établissement dans le référentiel ADS
=========================================================================================

*Identifiant* : ERP_ADS__MAJ_NUMERO_ERP_DOSSIER_AUTORISATION


*Cas d'utilisation* :

• Lors de l'ouverture de l'ERP, un numéro ERP est attribué au bâtiment. Cela occasionne une mise à jour du Numéro ERP dans le Référentiel d'Autorisations.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_autorisations_put>`


*Traitement* :

• Mise à jour des informations fournies sur le dossier d'autorisation : La mise à jour du champ `dossier_autorisation.erp_numero_batiment`.


*Contenu de l'échange* :

- **numero_erp** : c'est le code de l'établissement (exemple : 'T3498').


*Exemple* :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
    Host: localhost

    {
        "numero_erp":"T12345"
    }


.. _echange_erp_ads_202:

================================================================================================
[202](Échange ERP → ADS) Mise à jour du statut ouvert de l'établissement dans le référentiel ADS
================================================================================================

*Identifiant* : ERP_ADS__MAJ_STATUT_ERP_DOSSIER_AUTORISATION


*Cas d'utilisation* :

• Un arrêté d'ouverture ERP est signé. Cette information ainsi que la date sont transmis au logiciel ADS pour mise à jour du référentiel.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_autorisations_put>`


*Traitement* :

• Mise à jour des informations fournies sur le dossier d'autorisation : La mise à jour des champs `dossier_autorisation.erp_ouvert` et `dossier_autorisation.erp_date_ouverture`.


*Contenu de l'échange* :

• **erp_ouvert** : Marqueur signifiant l'ouverture de l'établissement (booléen : 'oui' / 'non').
• **date_arrete** : Date de la décision d'ouverture (Format : 12/01/2015). 


*Exemple* :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
    Host: localhost

    {
        "erp_ouvert":"oui",
        "date_arrete":"12/01/2015"
    }


.. _echange_erp_ads_203:

================================================================================
[203](Échange ERP → ADS) Récupération des informations depuis le référentiel ADS
================================================================================

*Identifiant* : ERP_ADS__RECUPERATION_INFORMATIONS_DOSSIER_AUTORISATION


*Cas d'utilisation* :

Le service ERP a besoin de consulter les informations contenues dans le Dossier d'Autorisation.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_autorisations_get>`


*Exemple* :

.. sourcecode:: http
      
    GET /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
    Host: localhost


.. _echange_erp_ads_204:

=======================================================================================
[204](Échange ERP → ADS) Dossier PC/ERP Information sur la complétude ERP Accessibilité
=======================================================================================

L'objectif principal de cet échange est de permettre aux services ERP d'apporter une réponse à l'échange [102] et d'informer l'instructeur ADS sur la complétude ERP du dossier.


*Identifiant* : ERP_ADS__PC__INFORMATION_COMPLETUDE_ERP_ACCESSIBILITE


*Cas d'utilisation* :

Le service ERP Accessibilité indique au service ADS si le dossier est complet ou pas. Un délai de 15 jours est prévu, mais n'est pas géré coté ADS : tous les messages provenant du logiciel ERP sont acceptés dans openADS, y compris hors délais. Pour pouvoir effectuer cette réponse le service ERP a accès aux pièces nécessaires du dossier ADS, cet accès n'est pas géré par openADS.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_messages_post>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme non lu.


*Contenu de l'échange* :

- **contenu** :

  • libelle « Complétude ERP ACC » : valeur : « oui/non »
  • libelle « Motivation Complétude ERP ACC » : valeur : texte libre multi-lignes


*Exemple* :

.. sourcecode:: http
      
    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ERP_ADS__PC__INFORMATION_COMPLETUDE_ERP_ACCESSIBILITE",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
            "Complétude ERP ACC": "non",
            "Motivation Complétude ERP ACC": "Lorem ipsum dolor sit amet..."
        }
    }


.. _echange_erp_ads_205:

==================================================================================
[205](Échange ERP → ADS) Dossier PC/ERP Information sur la complétude ERP Sécurité
==================================================================================

L'objectif principal de cet échange est de permettre aux services ERP d'apporter une réponse à l'échange [102] et d'informer l'instructeur ADS sur la complétude ERP du dossier.


*Identifiant* : ERP_ADS__PC__INFORMATION_COMPLETUDE_ERP_SECURITE


*Cas d'utilisation* :

• Le service ERP Sécurité indique au service ADS si le dossier est complet ou pas. Un délai de 15 jours est prévu, mais n'est pas géré coté ADS : tous les messages provenant du logiciel ERP sont acceptés dans openADS, y compris hors délais. Pour pouvoir effectuer cette réponse le service ERP a accès aux pièces nécessaires du dossier ADS, cet accès n'est pas géré par openADS.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_messages_post>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme non lu.


*Contenu de l'échange* :

- **contenu** :

  • libelle « Complétude ERP SECU » : valeur : « oui/non »
  • libelle « Motivation Complétude ERP SECU » : valeur : texte libre multi-lignes


*Exemple* :

.. sourcecode:: http
      
    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ERP_ADS__PC__INFORMATION_COMPLETUDE_ERP_SECURITE",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
            "Complétude ERP SECU": "oui",
            "Motivation Complétude ERP SECU": "Lorem ipsum dolor sit amet..."
        }
    }


.. _echange_erp_ads_206:

============================================================================
[206](Échange ERP → ADS) Dossier PC/ERP Information sur la qualification ERP
============================================================================

L'objectif principal de cet échange est de permettre aux services ERP d'apporter une réponse à l'échange [103] et d'informer l'instructeur ADS sur le caractère ERP du dossier.


*Identifiant* : ERP_ADS__PC__INFORMATION_QUALIFICATION_ERP


*Cas d'utilisation* :

Le service ERP répond à une demande de qualification d'un dossier ADS. Il renseigne le type et la catégorie ERP. Ces informations enrichiront le Référentiel Autorisations lorsqu'elles seront actualisées dans le Dossier d'Instruction par l'instructeur.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_messages_post>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme non lu.


*Contenu de l'échange* :

- **contenu** :

  • Confirmation ERP : oui/non (le Dossier est bien/n'est pas un ERP)
  • Type de dossier ERP : texte libre
  • Catégorie de dossier ERP : texte libre


*Exemple* :

.. sourcecode:: http
      
    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ERP_ADS__PC__INFORMATION_QUALIFICATION_ERP",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
            "Confirmation ERP": "oui",
            "Type de dossier ERP": "Lorem ipsum dolor sit amet...",
            "Catégorie de dossier ERP": "Lorem ipsum dolor sit amet..."
        }
    }


.. _echange_erp_ads_207:

============================================================================
[207](Échange ERP → ADS) Dossier PC/ERP Notification de dossier à enjeux ERP
============================================================================

L'objectif principal de cet échange est de permettre aux services ERP de partager le caractère 'à enjeu' du dossier pour en informer l'instructeur ADS.


*Identifiant* : ERP_ADS__PC__NOTIFICATION_DOSSIER_A_ENJEUX_ERP


*Cas d'utilisation* :

• Le service ERP peut qualifier le dossier comme Dossier à enjeux. Dans ce cas, un message « Dossier à enjeux ERP » est envoyé vers l'application ADS afin de mettre à jour le Dossier d'Instruction. La mise à jour est effectuée par l'instructeur ADS afin de s'assurer de la bonne prise en compte des répercussions de cette qualification pour l'instruction du dossier. Ce message ne met dons pas directement à jour le référentiel mais il est pris en compte dans les messages présentés à l'instructeur qui est chargé de mettre à jour ses données, et par voie de conséquence le référentiel. 


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_messages_post>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme non lu.


*Contenu de l'échange* :

- **contenu** :

  • Dossier à enjeux ERP : Oui / Non


*Exemple* :

.. sourcecode:: http
      
    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ERP_ADS__PC__NOTIFICATION_DOSSIER_A_ENJEUX_ERP",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
            "Dossier à enjeux ERP" : "oui"
        }
    }


.. _echange_erp_ads_208:

=================================================================================================
[208](Échange ERP → ADS) Dossier AT Mise à jour des informations arrêtées dans le référentiel ADS
=================================================================================================

*Identifiant* : ERP_ADS__AT__MAJ_ARRETE_ERP_DOSSIER_AUTORISATION


*Cas d'utilisation* :

• Lorsq'un arrêté d'autorisation de travaux est généré par les services ERP, l'information est transmise au référentiel ADS.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_autorisations_put>`


*Traitement* :

• Mise à jour des informations fournies sur le dossier d'autorisation : La mise à jour des champs `dossier_autorisation.erp_arrete_decision` et `dossier_autorisation.erp_date_arrete_decision`.


*Contenu de l'échange* :

• « arrete_effectue » : Arrêté effectué. Format : booléen (oui/non)
• « date_arrete » : Date de l'arrêté. Format : date (JJ/MM/YYYY)


*Exemple* :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
    Host: localhost

    {
        "arrete_effectue":"some",
        "date_arrete":"04/06/2014"
    }


.. _echange_erp_ads_209:

==============================================================
[209](Échange ERP → ADS) Dossier PC/ERP Retour de consultation
==============================================================

L'objectif principal de cet échange est de permettre aux services ERP de répondre à une consultation d'un instructeur ADS directement depuis openARIA (sans nécessité de le faire depuis l'interface dédiée aux services consultés dans openADS).


*Identifiant* : ERP_ADS__PC__RETOUR_DE_CONSULTATION


*Cas d'utilisation* :

• Le retour de consultation émise par l'instructeur est directement positionné par les services ERP.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_consultations_put>`


*Traitement* :

• Mise à jour de la consultation.


*Contenu de l'échange* :

• Date de retour d'avis (obligatoire) : {'date_retour': 'jj/mm/aaaa'} ;
• Avis (obligatoire) : {'avis' :'favorable|defavorable|favorable_reserve|...'} ;
• Motivation (facultatif) : {'motivation' :'Texte libre ...'} ;
• Nom du fichier de retour d'avis (facultatif) : {'nom_fichier' :'retour d'avis ABF.pdf'} ;
• Fichier encodé en base 64 (facultatif) : {'fichier_base64' :data}.


*Exemples* :

Retour d'avis d'une consultation sans fichier :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/consultations/12 HTTP/1.1
    Host: localhost

    {
        "date_retour": "14/01/2012",
        "avis": "Favorable"
    }

Retour d'avis d'une consultation avec fichier :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/consultations/12 HTTP/1.1
    Host: localhost

    {
        "date_retour": "14/01/2012",
        "avis": "Favorable",
        "fichier_base64": "JVBERi0xLjQKJcOkw7zDtsOfCjIgM",
        "nom_fichier": "plop.pdf"
    }


.. _echange_erp_ads_210:

===========================================================
[210](Échange ERP → ADS) Dossier AT Complétude Incomplétude
===========================================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est de mettre à jour l'information de complétude d'un dossier AT dans openADS suite à sa complétude/incomplétude dans openARIA pour que les agents du guichet unique puisse accomplir leur mission d'enregistrement des demandes correctement.


*Identifiant* : ERP_ADS__AT__MAJ_COMPLETUDE_INCOMPLETUDE


*Cas d'utilisation* :

• Ce message a vocation à permettre aux agents du Guichet unique de bien accomplir leur mission d'enregistrement face à l'arrivée d'une nouvelle pièce : si le dossier d'instruction AT est ouvert, alors les pièces sont acceptées (si le dossier est « incomplet », les pièces sont classées « complémentaires », sinon les pièces sont « supplémentaires ») et si le dossier est clos, les pièces sont refusées.
• Lorsque le dossier d'instruction d'AT est créé dans openADS, par défaut son statut doit être « complet ». Dès que la première incomplétude est faite dans openARIA, le message est envoyé.
• Le message de complétude doit mettre à jour automatiquement dans openADS le dossier d'instruction avec un statut complet, et cela doit se répercuter automatiquement sur le classement des nouvelles pièces arrivant au guichet unique.
• Importance du paramétrage du workflow des AT dans openADS.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_instructions_put>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Ajout d'un événement d'instruction


*Contenu de l'échange* :

• « message » : « complet » ou « incomplet »
• « date » : Date de la mise à jour de l'information au format JJ/MM/AAAA


*Exemple* :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/dossier_instructions/PC0130551600001P0 HTTP/1.1
    Host: localhost

    {
        "message":"complet",
        "date":"27/10/2013"
    }


.. _echange_erp_ads_211:

===========================================
[211](Échange ERP → ADS) Dossier AT Clôture
===========================================

Dans le contexte du guichet unique, l'objectif principal de cet échange est de mettre à jour l'information de clôture d'un dossier AT dans openADS suite à sa clôture dans openARIA pour que les agents du guichet unique puisse accomplir leur mission d'enregistrement des demandes correctement.

*Identifiant* : ERP_ADS__AT__MAJ_CLOTURE


*Cas d'utilisation* :

• Ce message a vocation à permettre aux agents du Guichet unique de bien accomplir leur mission d'enregistrement face à l'arrivée d'une nouvelle pièce : si le dossier d'instruction DAT est ouvert, alors les pièces sont acceptées (si le dossier est « incomplet », les pièces sont classées « complémentaires », sinon les pièces sont « supplémentaires ») et si le dossier est clos, les pièces sont refusées.
• Tous les dossiers d'instruction d'AT ne donnent pas lieu à un arrêté, ni même à une instruction. Vus du guichet unique et d'openADS ils peuvent donc toujours paraître « en cours d'instruction ». Dès que le dossier est clos dans openARIA pour Accessibilité et Sécurité, un message doit partir vers openADS.
• Le message de clôture doit mettre à jour automatiquement dans openADS le dossier d'instruction avec un statut « clos » et cela doit se répercuter automatiquement sur le refus des nouvelles pièces arrivant au guichet unique.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_instructions_put>`


*Traitement* :

• Création de message : Un message de catégorie "entrant" est ajouté dans openADS afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction. → Marqueur(s) de lecture du message : message marqué comme lu par défaut.
• Ajout d'un événement d'instruction


*Contenu de l'échange* :

• « message » : « clos » ou « ouvert »
• « date » : Date de la mise à jour de l'information au format JJ/MM/AAAA


*Exemple* :

.. sourcecode:: http
      
    PUT /openads/services/rest_entry.php/dossier_instructions/PC0130551600001P0 HTTP/1.1
    Host: localhost

    {
        "message":"clos",
        "date":"27/10/2013"
    }


.. _echange_erp_ads_212:

================================================================================
[212](Échange ERP → ADS) Récupération des informations depuis le référentiel ADS
================================================================================

*Identifiant* : ERP_ADS__RECUPERATION_INFORMATIONS_DOSSIER_INSTRUCTION


*Cas d'utilisation* :

Le service ERP a besoin de consulter les informations contenues dans le Dossier d'Instruction.


*Déclencheur* :

• :ref:`Web Service exposé<web_services_ressource_dossier_instructions_get>`


*Exemple* :

.. sourcecode:: http
      
    GET /openads/services/rest_entry.php/dossier_instructions/PC0130551601234P0 HTTP/1.1
    Host: localhost


.. _echange_erp_ads_213:

==================================================================================
[213](Échange ERP → ADS) Dossier PC Accusé de reception de consultation officielle
==================================================================================

L'objectif principal de cet échange est de notifier les instructeurs d'un dossier d'instruction du référentiel ADS que le dossier concerné va recevoir une consultation officielle pour avis ou pour conformité et leur permettre donc d'avoir une lettre type qui accuse la réception.

*Identifiant* : ERP_ADS__PC__AR_CONSULTATION_OFFICIELLE


*Cas d'utilisation* :

• Cette information est envoyée par le référentiel ERP au référentiel ADS suite à la notification de consultation officielle d'un dossier PC.

*Déclencheur* :

• Appel d'une méthode de maintenance par cron
• Le dossier est marqué comme « connecté au référentiel ADS »
• Un message de type :ref:`echange_ads_erp_104` ou :ref:`echange_ads_erp_106` a été reçu sur le dossier


*Traitement* :

• Création de message : Un message de catégorie "sortant" est ajouté dans openARIA afin de consigner l'échange. Il est visible depuis l'onglet "Message(s)" du dossier d'instruction et du dossier de coordination. → Marqueur(s) de lecture du message : mode 0.
• Envoi de la requête à destination de la ressource 'message' d'openADS. :ref:`Configuration des échanges sortants<configuration_echanges_sortants_referentiel_ads>`


*Contenu de l'échange* :

• « consultation » : l'identifiant de la consultation
• « date_reception » : Date de la reception de la consultation au format JJ/MM/AAAA


*Exemple* :

.. sourcecode:: http

    POST /openads/services/rest_entry.php/messages HTTP/1.1
    Host: localhost

    {
        "type": "ERP_ADS__PC__AR_CONSULTATION_OFFICIELLE",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
            "consultation" : 2,
            "date_reception": "16/06/2014 14:11"
        }
    }

