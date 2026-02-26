# 🔍 Projet Retail Expert Audit - PySpark

## 📋 Description

Système d'analyse et d'audit de données retail utilisant PySpark pour le traitement distribué de grandes volumétries de données commerciales.

## 🚀 Installation Automatique

Le projet inclut un script d'installation automatique qui :

1. **Met à jour les paquets système**
2. **Installe Java Runtime Environment (JRE)** - Version par défaut la plus stable
3. **Installe PySpark** - Framework de traitement distribué
4. **Détecte dynamiquement le chemin Java** - Compatible Google Colab et environnements variés
5. **Configure les variables d'environnement** automatiquement

### Script d'Initialisation

```python
# ==============================================================================
# INSTALLATION ET RÉSOLUTION DYNAMIQUE DE JAVA
# ==============================================================================
# 1. Installation de la version Java par défaut (la plus sûre)
!apt-get update -qq
!apt-get install -y default-jre &gt; /dev/null

# 2. Installation de PySpark
!pip install -q pyspark

import os
import subprocess

# 3. Recherche automatique du chemin d'installation exact de Java sur Colab
try:
    # Demande au système Linux où pointe exactement la commande "java"
    java_exec_path = subprocess.check_output(["readlink", "-f", "/usr/bin/java"]).decode("utf-8").strip()

    # Remonte de deux dossiers (depuis /bin/java vers le dossier parent) pour avoir le JAVA_HOME
    java_home = os.path.dirname(os.path.dirname(java_exec_path))

    # Fixation de la variable d'environnement avec le chemin réel trouvé
    os.environ["JAVA_HOME"] = java_home
    print(f"🔍 Chemin Java trouvé dynamiquement : {java_home}")

except Exception as e:
    print("❌ Impossible de trouver Java sur la machine :", e)

# 4. Lancement de PySpark
from pyspark.sql import SparkSession

try:
    spark = SparkSession.builder \
        .appName("Projet_Retail_Expert_Audit") \
        .master("local[*]") \
        .config("spark.sql.legacy.timeParserPolicy", "LEGACY") \
        .getOrCreate()



⚙️ Configuration Spark

| Paramètre                           | Valeur                       | Description                                |
| ----------------------------------- | ---------------------------- | ------------------------------------------ |
| `appName`                           | `Projet_Retail_Expert_Audit` | Nom de l'application                       |
| `master`                            | `local[*]`                   | Mode local avec tous les cœurs disponibles |
| `spark.sql.legacy.timeParserPolicy` | `LEGACY`                     | Compatibilité avec anciens formats de date |



| Problème             | Solution                                       |
| -------------------- | ---------------------------------------------- |
| `Java not found`     | Vérifier que `default-jre` est bien installé   |
| `Permission denied`  | Exécuter avec `sudo` si nécessaire             |
| `SparkSession error` | Vérifier la variable `JAVA_HOME` dans les logs |


📝 Notes

Le script utilise readlink -f pour résoudre les liens symboliques Java
La variable JAVA_HOME est définie dynamiquement selon l'environnement
Optimisé pour Google Colab mais fonctionne sur tout environnement Linux

👨‍💻 Auteur

BA Ibrahima, Mahamat
    print("✅ La SparkSession est initialisée avec succès.")
except Exception as e:
    print("❌ ERREUR au lancement de Spark :", e)
