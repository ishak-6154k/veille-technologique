
# Nom : KAYA #
# Prénom : ISHAK #
## Classe : BTS SIO SISR 1 ##
#### Date de naissance : 10/01/2007 ####
#### Etablissement d'origine : Lycée Camille Claudel 2022/2024 ####
#### Bac obtenu et spécialité : Bac général, spécialités mathématiques/NSI (physique-chimie abandonnée) ####
#### Année d'optention : 2025 ####
#### Avez-vous déjà effectué une autre formation après le bac ? : Non ####
#### Ville de résidence: Meulan/Les Mureaux, Les Yvelines ####
#### Moyen de transport : 25 min de voitures ####
#### Mail personnel : k.sharki6154@gmail.com ####
#### Pourquoi avoir choisi le BTS-SIO de L'Ensitech : Ce que je souhaite faire. ####
#### Travaillez-vous en dehors des cours : OUI ####
#### Quelles connaissances ou compétences en informatique avez-vous ? : J'ai pratiqué le langage python 
depuis 3ans, je connais Word et un peu Exel. J'ai déjà crée le tout début d'un site.
####
#### Projet Professionnel : BAC +5, Ingernieur en Cybersécurité ####
## Veille technologique : La Cryptographie Post-Quantique (PQC) et la sécurité des communications ##
###
L'émergence de l'informatique quantique représente une menace sans précédent pour les protocoles de chiffrement actuels.
Ma veille porte sur la transition vers la cryptographie post-quantique, qui vise à développer des systèmes cryptographiques 
sécurisés contre les ordinateurs classiques et quantiques. Je m'intéresse particulièrement aux standards du NIST et aux 
recommandations de l'ANSSI pour préparer la migration des infrastructures 
(VPN, certificats SSL/TLS) vers des algorithmes dits « résistants au quantique ».
###

import os
from datetime import datetime
import feedparser

# Sources axées sur la transformation de la donnée IoT et les enjeux de sécurité
RSS_FEEDS = {
    "IoT & Data Transformation": [
        "https://www.iotforall.com/feed",
        "https://internetofthingsagenda.techtarget.com/rss/ContentSyndication.xml",
    ],
    "Sécurité & Cybermenaces IoT": [
        "https://www.cisa.gov/cybersecurity-advisories/all.xml",
        "https://krebsonsecurity.com/feed/",
    ],
}


def build_markdown_report(feed_data):
    """Génère le rapport de veille structuré au format Markdown."""
    today = datetime.now().strftime("%Y-%m-%d")

    md = f"# Veille Technologique — Transformation des Données IoT & Sécurité\n\n"
    md += f"> **Dernière mise à jour automatique :** {today}\n\n"

    md += "## 🎯 Problématique\n"
    md += (
        "*Comment les objets connectés transforment-ils les données du quotidien "
        "en informations exploitables, et quels sont les enjeux de sécurité liés "
        "à leur collecte ?*\n\n"
    )

    md += "## 📊 Synoptique de la Chaîne de Valeur Data IoT\n"
    md += "| Étape | Mécanisme | Enjeu Sécurité Associé |\n"
    md += "| :--- | :--- | :--- |\n"
    md += "| **1. Collecte** | Capteurs bruts (température, pulsions, mouvement) | Interception, spoofing capteur |\n"
    md += "| **2. Traitement Edge** | Filtrage local, agrégation à la source | Firmware non sécurisé, accès physique |\n"
    md += "| **3. Transmission** | Protocoles légers (MQTT, CoAP, LoRaWAN) | Écoute clandestine (Eavesdropping), Man-in-the-Middle |\n"
    md += "| **4. Analyse Cloud / IA** | Modèles prédictifs, data visualization | Fuite de données massives, non-conformité RGPD |\n\n"

    md += "## 📰 Dernières Actualités et Analyses\n\n"

    for category, articles in feed_data.items():
        md += f"### {category}\n\n"
        if not articles:
            md += "_Aucun article récent récupéré._\n\n"
            continue

        for item in articles[:4]:  # Top 4 par catégorie
            md += f"- **[{item['title']}]({item['link']})**\n"
            if item.get("published"):
                md += f"  <br>_Publié le : {item['published']}_\n"
            md += "\n"

    md += "## 🛡️ Principaux Enjeux de Sécurité Identifiés\n\n"
    md += "- **Chiffrement de bout en bout** : Protéger la donnée de la capture au stockage.\n"
    md += "- **Gestion des identités (IAM)** : Authentification forte pour éviter le détournement d'équipements (ex. botnets IoT type Mirai).\n"
    md += "- **Mises à jour (OTA)** : Sécuriser la chaîne de correctifs à distance contre les failles d'injection.\n"
    md += "- **Confidentialité & RGPD** : Garantir l'anonymisation des données personnelles collectées au quotidien.\n"

    return md


def fetch_feeds():
    """Récupère les derniers articles depuis les flux RSS."""
    results = {}
    for category, urls in RSS_FEEDS.items():
        results[category] = []
        for url in urls:
            feed = feedparser.parse(url)
            for entry in feed.entries[:3]:
                results[category].append(
                    {
                        "title": entry.get("title", "Sans titre"),
                        "link": entry.get("link", "#"),
                        "published": entry.get("published", ""),
                    }
                )
    return results


def main():
    print("Récupération des données RSS...")
    data = fetch_feeds()
    report = build_markdown_report(data)

    with open("README.md", "w", encoding="utf-8") as f:
        f.write(report)

    print("README.md mis à jour avec succès !")


if __name__ == "__main__":
    main()


