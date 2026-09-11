
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

import csv
from collections import Counter

FICHIER = "donnees_quotidiennes.csv"


def charger_donnees():
    donnees = []

    try:
        with open(FICHIER, "r", encoding="utf-8") as fichier:
            lecteur = csv.DictReader(fichier)

            for ligne in lecteur:
                donnees.append(ligne)

    except FileNotFoundError:
        print(f"Erreur : le fichier {FICHIER} est introuvable.")
        return []

    return donnees


def afficher_resume(donnees):
    if not donnees:
        return

    print("\n" + "=" * 60)
    print("       ANALYSE DES DONNÉES D'UN SMARTPHONE")
    print("=" * 60)

    print(f"\nNombre d'événements analysés : {len(donnees)}")

    # Types de données
    types = Counter(d["type_donnee"] for d in donnees)

    print("\n--- TYPES DE DONNÉES COLLECTÉES ---")

    for type_donnee, nombre in types.items():
        print(f"{type_donnee:<25} : {nombre}")

    # Applications
    applications = Counter(d["application"] for d in donnees)

    print("\n--- APPLICATIONS CONCERNÉES ---")

    for application, nombre in applications.items():
        print(f"{application:<25} : {nombre}")

    # Niveau de sensibilité
    sensibilite = Counter(d["sensibilite"] for d in donnees)

    print("\n--- SENSIBILITÉ DES DONNÉES ---")

    for niveau, nombre in sensibilite.items():
        print(f"{niveau:<25} : {nombre}")

    # Données sensibles
    sensibles = [
        d for d in donnees
        if d["sensibilite"].lower() == "élevée"
    ]

    print("\n--- DONNÉES À RISQUE ---")
    print(f"Nombre de données sensibles : {len(sensibles)}")

    for donnee in sensibles:
        print(
            f"- {donnee['heure']} | "
            f"{donnee['type_donnee']} | "
            f"{donnee['application']}"
        )


def calculer_score(donnees):
    """
    Calcule un score fictif de sensibilité.
    Ce score sert uniquement à illustrer
    le niveau d'exposition des données.
    """

    points = {
        "faible": 1,
        "moyenne": 2,
        "élevée": 3
    }

    score = sum(
        points.get(d["sensibilite"].lower(), 0)
        for d in donnees
    )

    return score


def afficher_recommandations(donnees):
    print("\n" + "=" * 60)
    print("              RECOMMANDATIONS")
    print("=" * 60)

    print("""
1. Vérifier régulièrement les autorisations des applications.
2. Désactiver la localisation lorsqu'elle n'est pas nécessaire.
3. Utiliser l'authentification multifacteur.
4. Maintenir le smartphone et les applications à jour.
5. Éviter les réseaux Wi-Fi publics non sécurisés.
6. Utiliser des mots de passe uniques et complexes.
7. Limiter les applications ayant accès aux données personnelles.
""")


def main():
    donnees = charger_donnees()

    if not donnees:
        return

    afficher_resume(donnees)

    score = calculer_score(donnees)

    print("\n--- SCORE D'EXPOSITION ---")
    print(f"Score obtenu : {score}")

    if score <= 5:
        print("Niveau : FAIBLE")
    elif score <= 10:
        print("Niveau : MODÉRÉ")
    else:
        print("Niveau : ÉLEVÉ")

    afficher_recommandations(donnees)


if __name__ == "__main__":
    main()



