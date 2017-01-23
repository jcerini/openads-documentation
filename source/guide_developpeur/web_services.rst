.. _web_services_rest:

############
Web Services
############

Les web services d'openADS sont RESTful. Les retours sont au format JSON, encodés en UTF-8.

.. _web_services_ressource_maintenance:

Ressource "maintenance"
#######################

================================================
Synchronisation des utilisateurs via un annuaire
================================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "user"
      }

   **Exemples de réponse** :

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json; charset=utf-8

      {
        "http_code": 200,
        "http_code_message": "200 OK",
        "message": "Synchronisation terminée."
      }

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: application/json; charset=utf-8

      {
        "http_code": 200,
        "http_code_message": "200 OK",
        "message": "L'option 'annuaire' n'est pas configurée."
      }

   .. sourcecode:: http

      HTTP/1.1 500 Internal Server Error
      Content-Type: application/json; charset=utf-8

      {
        "http_code": 500,
        "http_code_message": "500 Internal Server Error",
        "message": "Erreur interne"
      }

   :statuscode 200: Tout s'est déroulé correctement.
   :statuscode 500: Erreur interne.


==============================================
Traitement des retours de consultation périmés
==============================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "consultation"
      }

   **Exemple de réponse** :

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Content-Type: text/javascript

      {
        "http_code": 200,
        "http_code_message": "200 OK",
        "message": "Aucune mise a jour."
      }



========================================
Traitement des événements suivant tacite
========================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "instruction"
      }



==========================================================
Importation des documents numérisés
==========================================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "import",
        "data": {
          // Ces deux paramètres sont facultatifs
          "Todo" : "chemin_dossier_source", // ou "" pour utiliser le chemin dans la configuration
          "Done" : "chemin_dossier_destination" // ou "" pour utiliser le chemin dans la configuration   
        }
      }



==========================================================
Purge des documents numérisés
==========================================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "purge",
        "data": {
          // Ces trois paramètres sont facultatifs
          "dossier": "chemin_dossier", // ou "" pour utiliser le chemin dans la configuration
          "nombre_de_jour": nombre_de_jour, // ou "" pour n'imposer aucunes limites,
          "dossier_vide" : true // ou false pour supprimer le répertoire si celui-ci est vide.
        }
      }



==========================================================
Mise à jour de l'état des dossiers d'autorisations périmés
==========================================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "update_dossier_autorisation",
      }



==========================================================
Synchronisation des contraintes depuis le SIG
==========================================================

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "contrainte",
      }


.. _web_services_ressource_maintenance_maj_metadonnees_documents_numerises:

=================================================
Mise à jour des métadonnées des pièces numérisées
=================================================

Ce web service déclenche le même traitement que 
:ref:`l'interface de mise à jour des
métadonnées.<parametrage_document_numerise_type_traiter_metadonnees>`

.. http:post:: /openads/services/rest_entry.php/maintenance

   **Exemple de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/maintenance HTTP/1.1
      Host: localhost

      {
        "module": "maj_metadonnees_documents_numerises",
      }



.. _web_services_ressource_consultations:

Ressource "consultations"
#########################

Cette ressource permet d'interfacer une consultation de service.

.. _web_services_ressource_consultations_put:

.. http:put:: /openads/services/rest_entry.php/consultations/(int:consultation_id)

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_209`

   **Exemple de requête** : Retour d'avis d'une consultation sans fichier

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/consultations/12 HTTP/1.1
      Host: localhost

      {
        "date_retour": "14/01/2012",
        "avis": "Favorable"
      }

   **Exemple de requête** : Retour d'avis d'une consultation avec fichier

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/consultations/12 HTTP/1.1
      Host: localhost

      {
        "date_retour": "14/01/2012",
        "avis": "Favorable",
        "fichier_base64": "JVBERi0xLjQKJcOkw7zDtsOfCjIgM",
        "nom_fichier": "plop.pdf"
      }



.. _web_services_ressource_dossier_autorisations:

Ressource "dossier_autorisations"
#################################

Cette ressource permet d'interfacer un dossier d'autorisation.

.. _web_services_ressource_dossier_autorisations_put:

.. http:put:: /openads/services/rest_entry.php/dossier_autorisations/(string:dossier_autorisation_id)

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_201`
   - :ref:`echange_erp_ads_202`
   - :ref:`echange_erp_ads_208`

   **Exemples de requête** :

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
      Host: localhost

      {
        "arrete_effectue":"some",
        "date_arrete":"04/06/2014"
      }

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
      Host: localhost

      {
        "erp_ouvert":"12345",
        "date_arrete":"some"
      }

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
      Host: localhost

      {
        "numero_erp":"12345",
        "avis":"some"
      }


.. _web_services_ressource_dossier_autorisations_get:

.. http:get:: /openads/services/rest_entry.php/dossier_autorisations/(string:dossier_autorisation_id)

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_203`

   **Exemple de requête** :

   .. sourcecode:: http
      
      GET /openads/services/rest_entry.php/dossier_autorisations/PC0130551601234 HTTP/1.1
      Host: localhost



.. _web_services_ressource_dossier_instructions:

Ressource "dossier_instructions"
################################

Cette ressource permet d'interfacer un dossier d'instruction.

.. _web_services_ressource_dossier_instructions_put:

.. http:put:: /openads/services/rest_entry.php/dossier_instructions/(string:dossier_instruction_id)

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_210`
   - :ref:`echange_erp_ads_211`

   **Exemples de requête** :

   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/dossier_instructions/PC0130551601234P0 HTTP/1.1
      Host: localhost

        {
            "message":"clos",
            "date":"27/10/2013"
        }


   .. sourcecode:: http
      
      PUT /openads/services/rest_entry.php/dossier_instructions/PC0130551601234P0 HTTP/1.1
      Host: localhost

        {
            "message":"complet",
            "date":"27/10/2013"
        }


.. _web_services_ressource_dossier_instructions_get:

.. http:get:: /openads/services/rest_entry.php/dossier_instructions/(string:dossier_instruction_id)

Les champs de premier niveau sont toujours présents dans le retour JSON, même si la valeur
est vide. Les champs de second niveau (ex: champs de données techniques, concernant une 
personne morale...) sont présents dans le retour JSON seulement s'ils sont applicables au
dossier.

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_212`

   **Exemple de requête sur dossier avec en pétionnaire principal une personne physique** :

   .. sourcecode:: http
      
      GET /openads/services/rest_entry.php/dossier_instructions/PC0130551601234P0 HTTP/1.1
      Host: localhost

        {
          "dossier_instruction": "PC0130551600001P0", // identifiant du dossier d'instruction
          "dossier_autorisation": "PC0130551600001", // identifiant du dossier d'autorisation
          "terrain_adresse_voie_numero": "10", // numéro de la voie de l'adresse du terrain
          "terrain_adresse_lieu_dit": "Les Baïsses", // lieu dit du terrain
          "terrain_adresse_code_postal": "13333", // code postal du terrain
          "terrain_adresse_cedex": "13366", // cedex de l'adresse du terrain
          "terrain_adresse_voie": "rue du 14 juillet", // voie de l'adresse du terrain
          "terrain_adresse_bp": "13380", // boite postale de l'adresse du terrain
          "terrain_adresse_localite": "Marseille", // ville de l'adresse du terrain
          "terrain_superficie": "22", // superficie du terrain en m²
          "references_cadastrales": [ // liste des références cadastrales
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0020"
            },
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0021"
            },
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0022"
            }
          ],
          "dossier_autorisation_type": "Permis de construire", // type de dossier d'autorisation
          "dossier_autorisation_type_detaille": "Permis de construire pour une maison individuelle et / ou ses annexes", // type de dossier d'autorisation détaillé
          "collectivite": "MARSEILLE", // commune qui instruit le dossier
          "instructeur": "Louis Laurent", // instructeur du dossier
          "division": "subdivision H", // division de l'instructeur du dossier
          "etat_dossier": "dossier rejeter manque de pieces", // état du dossier
          "statut_dossier": "cloture", // status du dossier
          "date_depot_initial": "2016-07-25", // date de dépôt du dossier
          "date_limite_instruction": "2016-09-25", // date limite d'instruction
          "date_decision": "2016-07-25", // date de décision du dossier (vide si pas de décision)
          "enjeu_urbanisme": "false", // dossier urbanisme à enjeu
          "enjeu_erp": "false", // dossier erp à enjeu
          "petitionnaire_principal": { // informations du pétitionnaire principal
            "demandeur": "13", // identifiant du pétitionnaire principal
            "qualite": "particulier", // type (particulier/personne_morale)
            "particulier_civilite": "Monsieur", 
            "particulier_nom": "LOUIS",
            "particulier_prenom": "Daniel",
            "particulier_date_naissance": "1982-10-20",
            "particulier_commune_naissance": "Puyricard",
            "particulier_departement_naissance": "13",
            "numero": "20",
            "voie": "rue du 14 juillet",
            "complement": "Bat A2",
            "lieu_dit": "Lambda",
            "localite": "Marseille",
            "code_postal": "13013",
            "bp": "13099",
            "cedex": "13010",
            "pays": "France",
            "division_territoriale": "DH3",
            "telephone_fixe": "0406042266",
            "telephone_mobile": "0622334123",
            "indicatif": "33",
            "courriel": "d.louis@wanadoo.fr",
            "fax": "0406042270"
          },
          "donnees_techniques": { // informations de certaines données techniques
            "co_tot_log_nb": "52", // nombre de logements créés
            "co_cstr_exist": "true", // changement de destination (true/false)
            "co_uti_pers": "true", // mode d’utilisation principale du logement : occupation personnelle ou en compte propre (true/false)
            "co_uti_vente": "true", // mode d’utilisation principale du logement : vente (true/false)
            "co_uti_loc": "true", // mode d’utilisation principale du logement : location (true/false)
            "su_tot_shon_tot": "40", // surface totale en m²
            "su_avt_shon_tot": "20", // surface existante avant traveaux en m²
            "am_lot_max_nb": "100", // le nombre maximum de lots prévus
            "am_empl_nb": "23" // le nombre maximum d’emplacements réservés aux tentes, caravanes ou résidences mobiles de loisirs
          }
        }

   **Exemple de requête sur dossier avec en pétionnaire principal une personne morale** :

   .. sourcecode:: http
      
      GET /openads/services/rest_entry.php/dossier_instructions/PC0130551601234P0 HTTP/1.1
      Host: localhost

        {
          "dossier_instruction": "PC0130551600001P0",
          "dossier_autorisation": "PC0130551600001",
          "terrain_adresse_voie_numero": "10",
          "terrain_adresse_lieu_dit": "Les Baïsses",
          "terrain_adresse_code_postal": "13333",
          "terrain_adresse_cedex": "13366",
          "terrain_adresse_voie": "rue du 14 juillet",
          "terrain_adresse_bp": "13380",
          "terrain_adresse_localite": "Marseille",
          "terrain_superficie": "22",
          "references_cadastrales": [
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0020"
            },
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0021"
            },
            {
              "prefixe": "202",
              "quartier": "810",
              "section": "A",
              "parcelle": "0022"
            }
          ],
          "dossier_autorisation_type": "Permis de construire",
          "dossier_autorisation_type_detaille": "Permis de construire pour une maison individuelle et / ou ses annexes",
          "collectivite": "MARSEILLE",
          "instructeur": "Louis Laurent",
          "division": "subdivision H",
          "etat_dossier": "dossier rejeter manque de pieces",
          "statut_dossier": "cloture",
          "date_depot_initial": "2016-07-25",
          "date_limite_instruction": "2016-09-25",
          "date_decision": "2016-07-25",
          "enjeu_urbanisme": "false",
          "enjeu_erp": "false",
          "petitionnaire_principal": {
            "demandeur": "13",
            "qualite": "personne_morale",
            "personne_morale_civilite": "Monsieur",
            "personne_morale_denomination": "Martin",
            "personne_morale_raison_sociale": "SARL",
            "personne_morale_siret": "13454566",
            "personne_morale_categorie_juridique": "SA",
            "personne_morale_nom": "LAFONT",
            "personne_morale_prenom": "Nicolas",
            "numero": "20",
            "voie": "rue du 14 juillet",
            "complement": "Bat A2",
            "lieu_dit": "Lambda",
            "localite": "Marseille",
            "code_postal": "13013",
            "bp": "13099",
            "cedex": "13010",
            "pays": "France",
            "division_territoriale": "DH3",
            "telephone_fixe": "0406042266",
            "telephone_mobile": "0622334123",
            "indicatif": "33",
            "courriel": "d.louis@wanadoo.fr",
            "fax": "0406042270"
          },
          "donnees_techniques": {
            "su_tot_shon_tot": "40",
            "su_avt_shon_tot": "20",
            "am_lot_max_nb": "100",
            "am_empl_nb": "23"
          }
        }

.. _web_services_ressource_messages:

Ressource "messages"
####################

Cette ressource permet d'interfacer un message.

.. _web_services_ressource_messages_post:

.. http:post:: /openads/services/rest_entry.php/messages

   **Cas d'utilisation** :

   - :ref:`echange_erp_ads_204` 
   - :ref:`echange_erp_ads_205` 
   - :ref:`echange_erp_ads_206` 
   - :ref:`echange_erp_ads_207` 
   - :ref:`echange_erp_ads_213` 

   **Exemples de requête** :

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/messages HTTP/1.1
      Host: localhost

        {
            "type": "Mise à jour de complétude ERP ACC",
            "date": "16/06/2014 14:12",
            "emetteur": "John Doe",
            "dossier_instruction": "PD12R0001",
            "contenu": {
                "Complétude ERP ACC": "non",
                "Motivation Complétude ERP ACC": "Lorem ipsum dolor sit amet..."
            }
        }

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/messages HTTP/1.1
      Host: localhost
      
        {
            "type": "Mise à jour de complétude ERP SECU",
            "date": "16/06/2014 14:12",
            "emetteur": "John Doe",
            "dossier_instruction": "PD12R0001",
            "contenu": {
                "Complétude ERP SECU": "oui",
                "Motivation Complétude ERP SECU": "Lorem ipsum dolor sit amet..."
            }
        }


   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/messages HTTP/1.1
      Host: localhost

      {
        "type": "Mise à jour de qualification",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
          "Confirmation ERP": "oui",
          "Type de dossier ERP": "Lorem ipsum dolor sit amet...",
                "Catégorie de dossier ERP": "Lorem ipsum dolor sit amet..."
            }
        } 

   .. sourcecode:: http
      
      POST /openads/services/rest_entry.php/messages HTTP/1.1
      Host: localhost

      {
        "type": "Dossier à enjeux ERP",
        "date": "16/06/2014 14:12",
        "emetteur": "John Doe",
        "dossier_instruction": "PD12R0001",
        "contenu": {
          "Dossier à enjeux ERP" : "oui"
        }
      }

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


