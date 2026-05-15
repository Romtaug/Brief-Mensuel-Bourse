# Assets

Ce dossier peut contenir ta musique personnalisée.

## 🎵 Ta propre musique

Si tu veux remplacer la musique par défaut (téléchargée automatiquement depuis Internet Archive) :

1. Trouve un MP3 royalty-free sur :
   - https://pixabay.com/music/ (filtre "Corporate" ou "Tech" — pas d'attribution requise)
   - https://www.youtube.com/audiolibrary (via YouTube Studio)
   - https://mixkit.co/free-stock-music/

2. Renomme le fichier en `music.mp3`

3. Place-le dans ce dossier `assets/music.mp3`

4. Commit + push :
   ```bash
   git add assets/music.mp3
   git commit -m "🎵 Add custom music"
   git push
   ```

Le script utilisera ton fichier en priorité s'il existe.

## ⚙️ Si pas de music.mp3

Le script télécharge automatiquement une musique royalty-free depuis Internet Archive (Adrian Diaz · CC BY-SA 4.0) avant la génération de la vidéo. Pas besoin d'action.
