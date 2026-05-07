# Customer-Personality-Analysis-BI
Projet BI end-to-end : SSIS + SQL Server + Power BI | Customer Personality Analysis

🚀 Customer Personality Analysis — Projet BI End-to-End

Projet complet de Business Intelligence : ETL avec SSIS → SQL Server → Dashboards Power BI
Dataset : Customer Personality Analysis — Kaggle


📌 Aperçu du projet
Ce projet couvre l'intégralité d'un pipeline data professionnel :
CSV Kaggle → SSIS (ETL) → SQL Server → Power BI (DAX + Dashboards)
2 240 clients · 29 attributs · 4 dashboards interactifs

🗂️ Structure du projet
📁 Customer-Personality-Analysis-BI/

│
├── 📁 SSIS/
│   └── Package.dtsx                  # Package SSIS (ETL complet)
│


├── 📁 SQL/
│   ├── create_table.sql              # Création de la table customer_data
│   ├── update_total_achat.sql        # Calcul Total_achat
│   ├── update_score_fidelite.sql     # Calcul Score_Fidélité
│   └── update_type_client.sql        # Segmentation Type_Client
│

├── 📁 PowerBI/
│   └── powerquery_complet.pbix       # Fichier Power BI complet
│
├── 📁 Screenshots/                   # Captures d'écran du projet
│
└── README.md

 Étape 1 — ETL avec SSIS
Pipeline Data Flow
[Flat File Source]  ──(2 240 rows)──►  [Derived Column]  ──(2 240 rows)──►  [OLE DB Destination]
  marketing.csv                          Transformations                     Customer_Analysis_DB
Colonnes dérivées calculées
Colonne dérivéeExpression SSISChildren_Total(DT_I4)Kidhome + (DT_I4)TeenhomeAge(DT_I4)YEAR(GETDATE()) - (DT_I4)Year_BirthCustomer_Seniority(DT_I4)ROUND(DATEDIFF("day",(DT_DBDATE)Dt_Customer,GETDATE()),0)TOTAL_DEPENSE(DT_R8)REPLACENULL(MntWines,0) + REPLACENULL(MntFruits,0) + ...
Destination OLE DB

Serveur : LocalHost
Base de données : Customer_Analysis_DB
Table : [dbo].[customer_data]


 Étape 2 — Calculs SQL post-chargement
Total_achat
sqlUPDATE customer_data
SET Total_achat = NumDealsPurchases 
                + NumWebPurchases 
                + NumCatalogPurchases 
                + NumStorePurchases;
Score_Fidélité
sqlUPDATE customer_data
SET Score_Fidelite = (Customer_Seniority * 0.4) 
                   + ((365 - Recency) * 0.3) 
                   + (TOTAL_DEPENSE / 1000 * 0.8);
Type_Client (Segmentation)
sqlUPDATE customer_data
SET Type_Client =
    CASE
        WHEN Total_achat >= 10 AND Recency <= 30 
            THEN 'Fidèle'
        WHEN Total_achat BETWEEN 5 AND 9 AND Recency <= 90 
            THEN 'Occasionnel'
        WHEN Recency > 180 
            THEN 'Inactif'
        ELSE 'À réactiver'
    END;
Résultat final — Colonnes enrichies
Children_TotalTOTAL_DEPENSECustomer_SeniorityAgeTotal_achatScore_FidélitéType_Client0161713682597,78À réactiver197213761796,09À réactiver1544127120114,46Fidèle113112369105,53Occasionnel

 Étape 3 — Dashboards Power BI
Modélisation

Table customer_data (source principale)
Table Dim_Date (calendrier)
Mesures DAX : CATotal, Taux Fidèles, Total Clients, Column

Pages du rapport 

Page                   |       Contenu

Améliorer les méventes |  Produits Stars · CA par Produit · Analyse par mois · Filtre Catégorie Age

Cycle de Vie Client    |  Risque perte clients · Taux Fidèles · Filtre Marital Status

  Solution livraison 
    à domicile         |  Solution livraison à domicile
    
   Marketing           |  Préférence Canal · Somme des produits par mois
                      

💡 Insights clés 

Insight                  |       Contenu

🍷 Produit star         |  Les Vins représentent +60% du CA total

⚠️ Risque perte         |  Risque perte clients · Taux Fidèles · Filtre Marital Status

🌐 Canal préféré       |  Le Web domine devant Magasin et Catalogue
    
🍂 Saisonnalité      |  L'Automne est la meilleure saison pour les achats catalogue

👴 Segment dominant  |  Les Seniors représentent 38,59% des clients
   
💡 Ce projet m'a permis de couvrir tout le pipeline data : 

ingestion → transformation → stockage → visualisation → insight.

🛠️ Stack : SSIS · SQL Server · Power BI · DAX · Power Query

📦 Dataset

Source : Kaggle — Customer Personality Analysis
Lignes : 2 240 clients
Colonnes : 29 attributs (démographie, produits, promotions, canaux)
