# Atelier-5
## Exercice 1
### Description 
Cet exercice est une page web affichant le profil d'un utilisateur ainsi que ses informations sous forme de tableau. L'affichage des données est simulé via des fonctions JavaScript asynchrones qui récupèrent les informations après un délai simulé.
### Fonctionnalités
=> Affichage d'un profil utilisateur (nom, email, genre, avatar)

=> Présentation des informations dans un tableau structuré

=> Simulation d'une récupération asynchrone des données utilisateur

=> Affichage des commandes utilisateur via une récupération asynchrone

=> Adaptation responsive du design pour différents écrans

### Structure du Code

#### 1. HTML:
Définition de la structure principale avec une section pour le profil utilisateur et un tableau pour afficher les informations.
#### 2. CSS:
=> Mise en page moderne et épurée

=> Utilisation de flexbox pour organiser l'affichage du profil

=> Styles adaptés aux écrans mobiles
#### 3. JavaScript:
L'utilisation de fonctions asynchrones permet de simuler une récupération de données comme si elles provenaient d'un serveur distant

=> **RecUserData()** : Fonction asynchrone qui retourne une promesse contenant les informations de l'utilisateur après un délai simulé de 3 secondes grâce à setTimeout.
```js
async function RecUserData(){
    return new Promise((resolve) => {
        setTimeout(() => {
            const userData = {
                name: 'John Doe',
                email: 'johndoe@example.com',
                avatar: 'avatar.png',
                gender: 'M',
                logine: 'JohnDoe',
                password: 'salma1',
                address: '512, boukhalef Tanger Maroc'
            };
            resolve(userData);
        }, 3000);
    });
}

```
 => **userprofile()** : Fonction asynchrone qui appelle RecUserData() et affiche les informations utilisateur.
```js
async function userprofile(){
    try {
        const Donnee = await RecUserData();
        console.log(Donnee);
        Afficheprofile(Donnee);
    } catch(error) {
        console.error("Erreur pour récupérer les données", error);
    }
}

async function userprofile(){
    try {
        const Donnee = await RecUserData();
        console.log(Donnee);
        Afficheprofile(Donnee);
    } catch(error) {
        console.error("Erreur pour récupérer les données", error);
    }
}
```
=> **Afficheprofile()** : Insère dynamiquement les informations utilisateur dans la section de profil HTML.

=> **afficheTable()** : Remplit le tableau HTML avec les informations utilisateur.

=> **AfficherUserTable()** : Fonction asynchrone qui récupère les données utilisateur via RecUserData() et met à jour l'affichage.

=> **getUserOrders()** : Fonction asynchrone qui simule la récupération d'une liste de commandes après un délai d'une seconde.
```js
function getUserOrders() {
    return new Promise((resolve) => {
        setTimeout(() => {
            const orders = ['Order 1', 'Order 2', 'Order 3'];
            resolve(orders);
        }, 1000);
    });
}
```
=> **fetchUserDataAndOrders()** : Fonction asynchrone qui récupère à la fois les informations utilisateur et ses commandes en attendant chaque réponse avant d'afficher les résultats.

<img src="https://github.com/user-attachments/assets/e5264cc1-5e93-4237-b8a4-ea2e0715cbfb" alt="Image" width="900" />

### Conclusion:

Cet exercice met en avant l'utilisation de JavaScript asynchrone pour simuler la récupération de données utilisateur de manière fluide et réaliste. Grâce aux fonctions asynchrones async/await, les informations sont chargées dynamiquement, offrant une meilleure expérience utilisateur sans bloquer l'interface.

## Exercice 2
### Description 
Cet exercice est une application simple permettant d'uploader et de récupérer des fichiers via une API développée en Laravel. L'interface utilisateur est une page HTML qui interagit avec l'API en utilisant JavaScript.
### Fonctionnalités
=> Téléversement de fichiers via un formulaire HTML.

=> Affichage des fichiers stockés sous forme de liste.

=> API Laravel pour gérer le stockage et la récupération des fichiers.
### Technologies utilisées
***Frontend*** : HTML, CSS, JavaScript.

***Backend*** : Laravel (PHP).

***Stockage des fichiers*** : Système de fichiers local avec Laravel Storage.
### Structure des fichiers
#### 1. index.html:
Ce fichier contient l'interface utilisateur pour uploader et afficher les fichiers.
```js
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Upload et Récupération de Fichiers</title>
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #6dd5ed, #2193b0);
            color: #fff;
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>
    <h1>Uploader un fichier</h1>
    <input type="file" id="fileInput">
    <button id="uploadBtn">Téléverser</button>
    <h2>Liste des fichiers</h2>
    <button id="fetchFilesBtn">Afficher les fichiers</button>
    <ul id="fileList"></ul>
    <script>
        document.getElementById('uploadBtn').addEventListener('click', async () => {
            const file = document.getElementById('fileInput').files[0];
            if (!file) {
                alert("Veuillez sélectionner un fichier.");
                return;
            }
            const formData = new FormData();
            formData.append('file', file);
            try {
                const response = await fetch('http://127.0.0.1:8000/api/upload', {
                    method: 'POST',
                    body: formData
                });
                const result = await response.json();
                alert(result.message);
            } catch (error) {
                alert("Erreur : " + error.message);
            }
        });
    </script>
</body>
</html>
```
#### 2. routes/api.php:
Ce fichier définit les routes de l'API.
```js
<?php
use App\Http\Controllers\FileController;
use Illuminate\Support\Facades\Route;

Route::post('/upload', [FileController::class, "upload"]);
Route::get('/files', [FileController::class, "getFiles"]);

```
#### 3. FileController.php:
Ce fichier gère le téléversement et la récupération des fichiers.
```js

<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Storage;

class FileController extends Controller
{
    public function upload(Request $request)
    {
        if ($request->hasFile('file')) {
            $file = $request->file('file');
            $filename = $file->getClientOriginalName();
            $path = $file->storeAs('uploads', $filename);
            return response()->json(['message' => 'Fichier téléchargé avec succès!', 'path' => $path]);
        }
        return response()->json(['message' => 'Aucun fichier trouvé.'], 400);
    }
    
    public function getFiles()
    {
        $files = Storage::files('uploads');
        return response()->json(['files' => $files]);
    }
}

```
### Utilisation
=> **Téléversement** : Sélectionner un fichier et cliquer sur "Téléverser"
<img src="https://github.com/user-attachments/assets/e7336442-abbe-4275-82f5-9699c22794aa" alt="Image" width="600" />

=> **Affichage des fichiers** : Cliquer sur "Afficher les fichiers" pour voir les fichiers stockés.Utilisation
<img src="https://github.com/user-attachments/assets/bb555892-d03d-464d-9e0d-688a5f7e7f55" alt="Image" width="600" />


## Exercice 3
### Description 
L'exercice "Gestion des Salles" permet de gérer les informations relatives aux salles d'un établissement. Il offre des fonctionnalités permettant d'ajouter, modifier, supprimer et afficher des salles via une API RESTful avec une interface utilisateur en HTML et JavaScript. Le backend utilise le framework Laravel, et le frontend est conçu avec HTML et JavaScript pour une interaction dynamique avec l'utilisateur.

### Technologies Utilisées
***Backend***
=>PHP avec le framework Laravel

=>Base de données : MySQL

=> API RESTful pour la gestion des ressources de salles

=> Validation des entrées utilisateur avec Laravel

***Frontend***
=> HTML5 et CSS3 pour la structure et le design

=> JavaScript pour la gestion dynamique de l'interface

=> Fetch API pour l'interaction avec l'API backend

### Structure du Projet
**1. Backend**
#### Fichier RoomController.php
Le contrôleur RoomController gère les opérations CRUD (Créer, Lire, Mettre à jour, Supprimer) pour les salles. Voici un résumé des actions disponibles dans ce contrôleur :

=> **index()** : Récupère la liste de toutes les salles.

=> **store(Request $request)** : Crée une nouvelle salle avec validation des données.

=> **show($id)** : Affiche une salle spécifique par son ID.

=> **update(Request $request, $id)** : Met à jour une salle existante avec de nouvelles données.

=> **destroy($id)** : Supprime une salle spécifique.
```js
public function store(Request $request)
{
    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'capacity' => 'required|integer|min:1',
    ]);

    $room = Room::create($validated);
    return response()->json(['message' => 'Salle sauvegardée avec succès!', 'room' => $room]);
}

public function update(Request $request, $id)
{
    $room = Room::findOrFail($id);

    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'capacity' => 'required|integer|min:1',
    ]);

    $room->update($validated);

    return response()->json(['message' => 'Salle mise à jour avec succès!', 'room' => $room]);
}


```

#### Fichier api.php
Définit les routes de l'API pour la gestion des salles :

=> **GET /api/rooms** : Liste toutes les salles

=> **GET /api/rooms/{id}** : Affiche les détails d'une salle

=> **POST /api/rooms** : Crée une nouvelle salle

=> **PUT /api/rooms/{id}** : Met à jour une salle existante

=> **DELETE /api/rooms/{id}** : Supprime une salle

```js
Route::get('rooms', [RoomController::class, 'index']);
Route::get('rooms/{id}', [RoomController::class, 'show']);
Route::post('rooms', [RoomController::class, 'store']);
Route::put('rooms/{id}', [RoomController::class, 'update']);
Route::delete('rooms/{id}', [RoomController::class, 'destroy']);

```

#### Migration create_room_table.php
Ce fichier crée la table rooms dans la base de données avec les colonnes nécessaires.
```js
public function up()
{
    Schema::create('rooms', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->integer('capacity');
        $table->timestamps();
    });
}

```
**2. Frontend**
***Fichier rooms.html***
Ce fichier fournit l'interface utilisateur pour l'ajout, la modification et la suppression des salles. Il inclut :

=> Formulaire d'ajout/modification : Permet à l'utilisateur de saisir le nom et la capacité d'une salle.

=> Liste des salles : Affiche la liste des salles enregistrées avec des boutons pour modifier ou supprimer chaque salle.

```js
<input type="text" id="name" placeholder="Nom de la salle">
<input type="number" id="capacity" placeholder="Capacité">
<button onclick="createOrUpdateRoom()">Sauvegarder</button>

<ul id="roomList"></ul>

```
**3. Styling (CSS)**
Le fichier CSS est conçu pour offrir une interface simple, moderne et responsive, avec des animations subtiles pour les éléments interactifs (boutons, champs de formulaire).

**4. Scripts JavaScript**
Le JavaScript gère les actions suivantes :

=> createOrUpdateRoom() : Crée ou met à jour une salle.

=> editRoom(room) : Pré-remplie le formulaire avec les données de la salle pour modification.

=> loadRooms() : Charge et affiche la liste des salles depuis l'API.

=> deleteRoom(id) : Supprime une salle via l'API.
```js
const apiUrl = 'http://127.0.0.1:8000/api/rooms';

function createOrUpdateRoom() {
    const id = document.getElementById('roomId').value;
    const name = document.getElementById('name').value;
    const capacity = document.getElementById('capacity').value;
    const room = { name, capacity };
    const method = id ? 'PUT' : 'POST';
    const url = id ? `${apiUrl}/${id}` : apiUrl;

    fetch(url, {
        method: method,
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(room)
    })
    .then(response => response.json())
    .then(data => { alert(data.message); loadRooms(); })
    .catch(error => console.error("Erreur :", error));
}

function loadRooms() {
    fetch(apiUrl)
        .then(res => res.json())
        .then(rooms => {
            const list = document.getElementById('roomList');
            list.innerHTML = '';
            rooms.forEach(room => {
                const li = document.createElement('li');
                li.innerHTML = `${room.name} (${room.capacity} places) <button onclick='editRoom(${JSON.stringify(room)})'>Modifier</button>`;
                list.appendChild(li);
            });
        })
        .catch(error => console.error("Erreur :", error));
}

```
### Fonctionnalités principales
***API RESTful***
=> **GET /api/rooms**: Récupérer toutes les salles.

=> **GET /api/rooms/{id}** : Récupérer une salle spécifique.

=> **POST /api/rooms**: Ajouter une nouvelle salle.

=> **PUT /api/rooms/{id}** : Mettre à jour les informations d'une salle.

=> **DELETE /api/rooms/{id}** : Supprimer une salle.

***Frontend***
==> Interface utilisateur pour ajouter, modifier et supprimer des salles.
<img src="https://github.com/user-attachments/assets/c8221e76-182f-4d95-9a59-47dc07324f60" alt="Image" width="650" />

=> Liste des salles affichée avec les options d'édition et de suppression.
<img src="https://github.com/user-attachments/assets/b7b5ab31-57d6-4fa0-a5ad-c308ccd83f01" alt="Image" width="650" />
### Conclusion
Cet exercice permet de gérer facilement les salles avec une API RESTful et une interface simple pour l'administration via un frontend en HTML et JavaScript. Les actions de CRUD sur les salles sont toutes prises en charge par l'API Laravel.

