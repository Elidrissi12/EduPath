# Analyse du Projet G5 - Microservices EduPath-MS

## 📋 Vue d'ensemble

**EduPath-MS** (Learning Analytics & Recommandations) est une plateforme d'apprentissage intelligent conçue pour améliorer la qualité de l'enseignement et optimiser l'accompagnement des étudiants grâce à une architecture microservices distribuée. Elle combine des services métier développés en **Spring Boot** et un moteur d'intelligence artificielle implémenté en **FastAPI**, chargé d'effectuer l'analyse prédictive des performances académiques et de générer des recommandations pédagogiques personnalisées.

### Équipe de Développement
- **HEDDAJI Malika**
- **LEMKHANTAR Abdelmoughith**
- **ZAKI EL IDRISSI Abdallah**
- **ELKHASSIBI Khawla**
- **TAOUFIK Mohamed Amine**

**Institution** : École Marocaine des Sciences de l'Ingénieur (EMSI), Membre de Honoris United Universities, Maroc

---

## 🏗️ Architecture Globale

### Pattern Architectural
- **Architecture Microservices** avec Spring Cloud
- **Service Discovery** : Eureka Server
- **Configuration Centralisée** : Spring Cloud Config Server
- **API Gateway** : Point d'entrée unique pour toutes les requêtes externes
- **Persistance** : **Fichiers CSV** (solution légère, portable et facilement exploitable)
- **Machine Learning** : Service FastAPI séparé (EduPath ML) pour l'IA
- **Frontend** : Application React conteneurisée

### Innovation Clé
**⚠️ IMPORTANT** : Contrairement aux plateformes traditionnelles, EduPath-MS utilise **exclusivement des fichiers CSV** pour la persistance des données pédagogiques, évitant la complexité des bases de données relationnelles. Cette approche permet :
- Réduction des coûts d'infrastructure
- Portabilité élevée
- Facilité d'exploitation pour l'analyse et l'entraînement des modèles d'IA
- Interopérabilité avec d'autres outils (Excel, Python, outils statistiques)

---

## 🔧 Services Identifiés

### 1. **Config Server** (Port 8888)
- **Rôle** : Serveur de configuration centralisé
- **Technologie** : Spring Cloud Config
- **Dépendances** : Aucune (service de base)
- **Healthcheck** : Configuré avec curl

### 2. **Eureka Server** (Port 3334)
- **Rôle** : Service de découverte et enregistrement des microservices
- **Technologie** : Netflix Eureka
- **Dépendances** : Config Server (doit être démarré en premier)
- **Healthcheck** : Configuré avec curl sur `/eureka/`

### 3. **API Gateway** (Port 8089)
- **Rôle** : Point d'entrée unique pour toutes les requêtes
- **Technologie** : Spring Cloud Gateway (probablement)
- **Dépendances** : Config Server, Eureka Server
- **Fonctionnalités** : Routage, load balancing, authentification centralisée

### 4. **User Service** (Port 8081)
- **Rôle** : Gestion des utilisateurs (étudiants et enseignants)
- **Technologie** : Spring Boot
- **Persistance** : Fichiers CSV
- **Dépendances** : Config Server, Eureka Server
- **Fonctionnalités** :
  - Création, consultation et mise à jour des profils
  - Authentification et autorisation
  - Gestion des rôles (étudiants/enseignants)

### 5. **Course Service** (Port 8082)
- **Rôle** : Gestion des cours, modules et ressources pédagogiques
- **Technologie** : Spring Boot
- **Persistance** : Fichiers CSV
- **Dépendances** : Config Server, Eureka Server
- **Fonctionnalités** :
  - Organisation des cours et modules
  - Gestion du contenu pédagogique
  - Catalogue de cours

### 6. **Activity Service** (Port 8083)
- **Rôle** : Collecte et stockage des activités d'apprentissage
- **Technologie** : Spring Boot
- **Persistance** : Fichiers CSV
- **Dépendances** : Config Server, Eureka Server
- **Fonctionnalités** :
  - Enregistrement des interactions et de la progression
  - Suivi des activités d'apprentissage (temps passé, taux de complétion, résultats aux quiz)
  - Historique et traçabilité des données

### 7. **Analytics Service** (Port 8084)
- **Rôle** : Agrégation des données et production d'indicateurs
- **Technologie** : Spring Boot
- **Persistance** : Lecture des fichiers CSV
- **Dépendances** : Config Server, Eureka Server
- **Fonctionnalités** :
  - Calcul d'indicateurs académiques
  - Statistiques globales
  - Tableaux de bord analytiques
  - Visualisation des performances

### 8. **AI Service (EduPath ML)** (Port 8000)
- **Rôle** : Intelligence artificielle et recommandations personnalisées
- **Technologie** : **FastAPI** (Python 3.11)
- **Dépendances** : Aucune explicite
- **Fonctionnalités** :
  - **Prédiction de la réussite académique** (modèle XGBoost)
  - **Identification des étudiants à risque**
  - **Génération de recommandations ciblées** :
    - Révisions des chapitres fondamentaux
    - Proposition de ressources complémentaires
    - Ajustement du rythme d'apprentissage
  - Analyse prédictive des performances

### 9. **Frontend** (Port 3000)
- **Rôle** : Interface utilisateur web moderne
- **Technologie** : **React**
- **Dépendances** : API Gateway, EduPath ML
- **Fonctionnalités** :
  - Interface pour étudiants et enseignants
  - Consultation des recommandations
  - Visualisation des statistiques et performances académiques
  - Tableaux de bord personnalisés
  - Gestion des cours et évaluations

---

## 🗄️ Architecture de Persistance

### Persistance par Fichiers CSV

**Approche innovante** : EduPath-MS utilise **exclusivement des fichiers CSV** pour stocker toutes les données pédagogiques, évitant ainsi la complexité et les coûts d'une base de données relationnelle.

#### Avantages de cette approche :
- ✅ **Légèreté** : Solution portable et facilement exploitable
- ✅ **Interopérabilité** : Format standard compatible avec Excel, Python, outils statistiques
- ✅ **Simplicité** : Pas de gestion de SGBD, réduction de la complexité technique
- ✅ **Coût réduit** : Pas de coûts d'hébergement et d'administration de base de données
- ✅ **Facilité d'analyse** : Données directement exploitables pour l'entraînement des modèles ML

#### Structure des fichiers CSV :
- `student_info_normalized.csv` : Informations démographiques et d'inscription
- `student_vle_normalized.csv` : Interactions VLE (Virtual Learning Environment)
- `student_assessment_normalized.csv` : Résultats aux évaluations
- `registrations_normalized.csv` : Inscriptions aux modules
- `assessments_normalized.csv` : Définitions des évaluations
- `courses_normalized.csv` : Informations sur les modules
- `vle_info_normalized.csv` : Ressources pédagogiques

**Note** : Le conteneur MySQL présent dans `docker-compose.yml` pourrait être utilisé pour d'autres besoins (logs, métadonnées système), mais les données métier sont stockées en CSV.

---

## 🔄 Flux de Communication

```
Frontend (3000)
    ↓
API Gateway (8089)
    ↓
    ├──→ User Service (8081) ←→ MySQL
    ├──→ Course Service (8082) ←→ MySQL
    ├──→ Activity Service (8083) ←→ MySQL
    ├──→ Analytics Service (8084)
    └──→ EduPath ML (8000)

Tous les services s'enregistrent auprès de:
    Eureka Server (3334)
    
Tous les services récupèrent leur config depuis:
    Config Server (8888)
```

---

## 📦 Structure du Projet

### Organisation des Dossiers

```
G5-projet-microservices/
├── docker-compose.yml              # Orchestration Docker
├── .gitmodules                     # Sous-modules Git
│
├── Infrastructure Services
│   ├── spring-config-global/      # Config Server
│   ├── eureka-server-global/      # Eureka Server
│   └── gateway-global/            # API Gateway
│
├── Business Services
│   ├── user-service-global/       # User Service
│   ├── course-service/            # Course Service
│   ├── activity-service/          # Activity Service
│   └── analytics-service-global/  # Analytics Service
│
├── ML Service
│   └── EduPath/                   # Service Machine Learning
│
├── Frontend
│   ├── Edu-path-project/          # Frontend principal
│   ├── frontend-edupath/          # Alternative frontend
│   └── front-end/                 # Autre frontend
│
└── Configuration
    └── microservices-config/      # Configurations partagées
```

**Note** : Le projet utilise des **sous-modules Git** pour gérer les différents services de manière indépendante.

---

## 🔐 Configuration et Sécurité

### Variables d'Environnement
- **SPRING_CLOUD_CONFIG_URI** : Point d'accès au Config Server
- **SPRING_CLOUD_CONFIG_FAILFAST** : Gestion des erreurs de configuration
- **SPRING_DATASOURCE_URL** : URLs de connexion MySQL
- **MYSQL_ROOT_PASSWORD** : `root` (⚠️ À changer en production)

### Healthchecks
- Config Server : Vérification HTTP sur le port 8888
- Eureka Server : Vérification HTTP sur `/eureka/`
- Intervalles : 5 secondes
- Timeout : 3 secondes
- Retries : 6 tentatives

---

## 🚀 Déploiement et CI/CD

### Pipeline DevOps

Le projet utilise un pipeline DevOps complet avec intégration continue et déploiement automatisé :

```
Développeur (Code Source)
    ↓
GitHub Repository
    ↓
GitHub Actions / Jenkins (CI/CD Pipeline)
    ↓
Build Images Docker (Versionnées)
    ↓
VPS Hostinger (Ubuntu 24 LTS)
    ↓
Docker Compose (Orchestration)
    ↓
Microservices EduPath-MS (Déployés)
```

### Étapes du Pipeline CI/CD

1. **Clonage du dépôt Git**
2. **Compilation et tests automatisés**
3. **Construction des images Docker** (versionnées : latest, v1.0, v1.1, etc.)
4. **Publication sur registre Docker**
5. **Déploiement automatisé sur VPS**
6. **Vérification du bon fonctionnement** (healthchecks)

### Déploiement sur VPS Hostinger

- **Environnement** : VPS Hostinger, Ubuntu 24 LTS
- **Avantages** :
  - Contrôle total de l'environnement système
  - Montée en charge progressive
  - Coût d'hébergement réduit
  - Compatibilité optimale avec Docker et Java LTS
- **Persistance** : Volumes Docker pour les fichiers CSV

### Démarrage des Services

L'ordre de démarrage est géré par Docker Compose avec `depends_on` :

1. **Config Server** (démarre en premier)
2. **Eureka Server** (attend que Config Server soit healthy)
3. **API Gateway** (attend Config + Eureka)
4. **Services métier** (attendent Config + Eureka)
5. **EduPath ML** (service indépendant)
6. **Frontend** (attend API Gateway + ML Service)

### Commandes Docker
```bash
# Démarrer tous les services
docker-compose up -d

# Voir les logs
docker-compose logs -f

# Arrêter tous les services
docker-compose down

# Reconstruire les images
docker-compose build
```

### Versionnement des Images

- `latest` : Version stable courante
- `v1.0`, `v1.1`, etc. : Versions figées
- Permet le rollback rapide en cas d'erreur
- Traçabilité des versions déployées

---

## ⚠️ Points d'Attention et Recommandations

### 1. **Sécurité**
- ❌ Mot de passe MySQL en dur (`root/root`)
- ✅ **Recommandation** : Utiliser des secrets Docker ou variables d'environnement sécurisées
- ✅ **Recommandation** : Implémenter HTTPS pour les communications inter-services

### 2. **Persistance CSV**
- ✅ Approche innovante et légère
- ⚠️ **Limitation** : Performance potentiellement limitée avec très gros volumes
- ⚠️ **Limitation** : Pas de transactions ACID
- ✅ **Recommandation** : Implémenter des backups automatiques des fichiers CSV
- ✅ **Recommandation** : Considérer une migration vers une base de données pour la production à grande échelle

### 3. **Résilience**
- ⚠️ Pas de circuit breaker visible dans la configuration
- ✅ **Recommandation** : Implémenter Resilience4j ou Hystrix pour la gestion des pannes
- ✅ **Recommandation** : Ajouter des timeouts et retry policies

### 4. **Monitoring**
- ⚠️ Pas de service de monitoring visible (Prometheus, Grafana)
- ✅ **Recommandation** : Ajouter un système de monitoring et logging centralisé
- ✅ **Recommandation** : Implémenter des métriques de performance

### 5. **Configuration**
- ✅ Utilisation de Spring Cloud Config (bonne pratique)
- ⚠️ **Recommandation** : Vérifier que les configurations sensibles ne sont pas en clair

### 6. **Structure du Projet**
- ⚠️ Présence de dossiers dupliqués (`EduPath`, `EduPath2`, `front-end`, `frontend-edupath`)
- ✅ **Recommandation** : Nettoyer les dossiers non utilisés
- ✅ **Recommandation** : Documenter quel frontend est actif

### 7. **Sous-modules Git**
- ⚠️ Les sous-modules semblent non initialisés (dossiers vides)
- ✅ **Recommandation** : Initialiser les sous-modules avec `git submodule update --init --recursive`

---

## 📊 Stack Technologique

### Backend
- **Java 21** (ou Java 17 LTS)
- **Spring Boot** : Framework pour les microservices métier
- **Spring Cloud** :
  - Spring Cloud Config (configuration centralisée)
  - Netflix Eureka (service discovery)
  - Spring Cloud Gateway (API Gateway)
- **Maven** : Gestion des dépendances

### Machine Learning & IA
- **Python 3.11**
- **FastAPI** : Framework moderne pour le service d'IA
- **XGBoost** : Modèle de machine learning pour la prédiction
- **Bibliothèques ML** : Pandas, NumPy, Scikit-learn

### Frontend
- **React** : Bibliothèque JavaScript pour l'interface utilisateur
- **Node.js 18+** : Environnement d'exécution
- **npm** : Gestionnaire de paquets

### Infrastructure & DevOps
- **Docker** : Conteneurisation des services
- **Docker Compose** : Orchestration des conteneurs
- **GitHub Actions** : Pipeline CI/CD
- **Jenkins** : Pipeline CI/CD alternatif
- **Git Submodules** : Gestion multi-repositories
- **VPS Hostinger** : Hébergement (Ubuntu 24 LTS)

### Versioning & Licence
- **Git** : Contrôle de version
- **MIT License** : Licence open source

---

## 🎯 Fonctionnalités Principales

### 1. **Gestion des Utilisateurs**
- Création, consultation et mise à jour des profils étudiants et enseignants
- Authentification et autorisation
- Gestion des rôles et permissions

### 2. **Gestion des Cours**
- Organisation des cours et modules pédagogiques
- Catalogue de cours
- Gestion des ressources pédagogiques

### 3. **Suivi des Activités d'Apprentissage**
- Enregistrement automatique des interactions (connexions, progression, scores)
- Suivi de la progression dans les cours
- Historique et traçabilité des données d'apprentissage

### 4. **Analyse des Performances**
- Calcul d'indicateurs académiques à partir des fichiers CSV
- Statistiques globales et agrégées
- Tableaux de bord analytiques pour enseignants

### 5. **Recommandations Personnalisées (IA)**
- **Prédiction de la réussite académique** avec probabilité de réussite
- **Identification des étudiants à risque** (risque faible/moyen/élevé)
- **Génération de recommandations ciblées** :
  - Révisions des chapitres fondamentaux
  - Proposition de ressources complémentaires (vidéos, exercices)
  - Ajustement du rythme d'apprentissage
  - Suggestions d'accompagnement pédagogique

### 6. **Tableaux de Bord**
- Tableau de bord étudiant : recommandations, progression, performances
- Tableau de bord enseignant : vue globale de la classe, étudiants à risque
- Tableau de bord IA : visualisation des prédictions et métriques

### 7. **API REST Documentée**
- Intégration facilitée avec d'autres plateformes éducatives
- Endpoints REST pour tous les services

## 📊 Modèle d'Intelligence Artificielle

### Dataset OULAD
- **Source** : Open University Learning Analytics Dataset (Kaggle)
- **Volume** : **32 593 étudiants**
- **Fichiers** :
  - 10 655 280 interactions VLE
  - 173 912 évaluations
  - 22 modules
  - 6 364 ressources pédagogiques

### Modèle XGBoost
- **Algorithme** : eXtreme Gradient Boosting
- **Tâche** : Classification binaire (Réussite/Échec)
- **Features** : **15 variables** regroupées en 4 catégories :
  1. **Démographiques et académiques** : code_module, highest_education, age_band, etc.
  2. **Engagement VLE** : click_total, click_mean, click_std
  3. **Performance académique** : score_mean, score_std, score_min, score_max
  4. **Historique** : num_of_prev_attempts, studied_credits

### Performances du Modèle
- **Accuracy** : **80.32%**
- **Precision** : 0.82
- **Recall** : 0.79
- **F1-Score** : 0.80
- **AUC-ROC** : **0.85** (excellente capacité de discrimination)

### Pipeline de Traitement
1. **Normalisation** (LMS Connector) : Préparation des données brutes
2. **Feature Engineering** (Prepa Data) : Création des features
3. **Profilage** (Student Profiler) : Analyse des profils étudiants
4. **Prédiction** (Path Predictor) : Génération des prédictions et recommandations

---

## 📈 Scénarios d'Utilisation

### Scénario 1 : Prédiction du Risque d'Échec
1. **Collecte** : Les activités de l'étudiant sont enregistrées dans des fichiers CSV
2. **Analyse** : Le service AI analyse les indicateurs et calcule une probabilité de réussite (ex: 42%)
3. **Interprétation** : Score interprété comme "risque élevé d'échec"
4. **Restitution** : Prédiction affichée sur le tableau de bord étudiant

### Scénario 2 : Recommandations Personnalisées
1. **Lecture** : Le service lit les fichiers CSV historiques
2. **Génération** : Le moteur IA génère des recommandations ciblées
3. **Livraison** : Affichage dans l'espace personnel de l'étudiant

### Scénario 3 : Suivi Pédagogique pour Enseignants
1. **Agrégation** : Tableau de bord agrège les données de tous les étudiants
2. **Visualisation** : Taux de réussite, répartition des risques, évolution temporelle
3. **Aide à la décision** : Identification des étudiants nécessitant un accompagnement prioritaire
4. **Export** : Résultats exportables en CSV pour analyse complémentaire

## 📝 Prochaines Étapes Recommandées

1. **Initialiser les sous-modules Git**
   ```bash
   git submodule update --init --recursive
   ```

2. **Enrichir les données d'apprentissage**
   - Participation aux forums
   - Régularité de connexion
   - Interactions avec ressources multimédias

3. **Améliorer la précision des prédictions**
   - Augmenter le volume des données historiques
   - Tester des modèles plus avancés (réseaux neuronaux, modèles hybrides)
   - Optimiser les hyperparamètres

4. **Personnalisation avancée**
   - Prendre en compte le style d'apprentissage
   - Recommandations basées sur l'historique de réussite

5. **Robustesse et montée en charge**
   - Tests de charge pour évaluer la capacité avec volumes croissants
   - Optimisation des temps de réponse

6. **Validation opérationnelle**
   - Déploiements pilotes dans différents contextes pédagogiques
   - Collecte de métriques de validation
   - Mécanisme de feedback utilisateur

7. **Extensions futures**
   - Application mobile
   - Intégration avec Moodle, Google Classroom
   - Modèle SaaS éducatif

---

## 📌 Conclusion

**EduPath-MS** constitue une **contribution significative** dans le domaine du Learning Analytics en proposant une plateforme intelligente, modulaire et légère pour le suivi des performances académiques et la génération de recommandations personnalisées.

### Contributions Principales

1. **Architecture microservices éducative** favorisant modularité, maintenabilité et évolutivité
2. **Pipeline d'analyse d'apprentissage de bout en bout** (collecte → analyse → recommandations)
3. **Moteur d'IA basé sur FastAPI** avec modèle XGBoost (Accuracy 80.32%, AUC-ROC 0.85)
4. **Approche innovante de persistance CSV** évitant la complexité des bases de données
5. **Système de recommandations personnalisées** orienté soutien pédagogique
6. **Intégration DevOps complète** (GitHub Actions, Jenkins, Docker)

### Points Forts

- ✅ Architecture microservices claire et bien structurée
- ✅ Service discovery et configuration centralisée
- ✅ Séparation des préoccupations (ML, Frontend, Backend)
- ✅ Approche légère et portable (CSV)
- ✅ Modèle ML performant (XGBoost avec AUC-ROC 0.85)
- ✅ Pipeline CI/CD automatisé
- ✅ Déploiement reproductible avec Docker

### Points à Améliorer

- ⚠️ Sécurité (mots de passe, HTTPS)
- ⚠️ Monitoring et observabilité (Prometheus, Grafana)
- ⚠️ Résilience et gestion des pannes (Circuit Breaker)
- ⚠️ Validation opérationnelle à grande échelle
- ⚠️ Performance avec très gros volumes de données CSV

### Impact Attendu

EduPath-MS démontre le potentiel de l'IA et des architectures microservices appliquées au domaine éducatif. Les bénéfices attendus incluent :
- **Amélioration du taux de réussite** grâce aux recommandations personnalisées
- **Détection précoce** des étudiants à risque
- **Réduction de l'échec scolaire** et de l'abandon
- **Optimisation des interventions éducatives**
- **Transformation numérique** des pratiques pédagogiques

Le projet illustre qu'une solution distribuée combinant Spring Boot et FastAPI peut offrir des résultats pertinents en matière de prédiction de réussite et d'aide à la décision pédagogique, tout en restant simple à déployer et à maintenir.

---

## 📚 Références

- **Repository principal** : https://github.com/abdelmoughith/G5-projet-microservices
- **Gateway** : https://github.com/abdelmoughith/gateway-global
- **Documentation** : https://github.com/abdelmoughith/G5-projet-microservices
- **Contact** : contact@edupath-ms.com
- **Licence** : MIT License
- **Version** : v1.0

