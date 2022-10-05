# Sommaire
- [Avant-propos](#avant-propos)
- [Notes de Joe Penna](#notes-by-joe-penna)
- [Installation](#setup)
  - [Instructions d'utilisation avec runpod](#easy-runpod-instructions)
  - [Installation avec Vast.AI](#vast-ai-setup)
- [Textual Inversion vs. Dreambooth](#text-vs-dreamb)
- [Utiliser le modèle généré](#using-the-generated-model)
- [Debugger vos résultats](#debugging-your-results)
  - [Ils ne vous ressemblent pas du tout!](#they-dont-look-like-you)
  - [Ils vous ressemblent un peu, mais exactement comme vos images d'entrainement](#they-sorta-look-like-you-but-exactly-like-your-training-images)
  - [Ils vous ressemblent, mais pas quand vous essayez avec d'autres styles](#they-look-like-you-but-not-when-you-try-different-styles)
- [Hugging Face Diffusers](#hugging-face-diffusers)

# Le Repo anciennement connu sous le nom de "Dreambooth"
## ...maintenant plus précisément décrit ainsi: "Unfrozen Model Textual Inversion for Stable Diffusion"
![image](https://user-images.githubusercontent.com/100188076/192390551-cb89364f-af57-4aed-8f3d-f9eb9b61cf95.png)

## <a name="avant-propos"></a>  Avant-propos de Léo

Salut, c'est Léo ([@Desp_OnChain](https://twitter.com/Desp_OnChain) sur Twitter), merci de t'intéresser à ce que je fais.

Ce repo est tiré du repo de JoePenna: https://github.com/JoePenna/Dreambooth-Stable-Diffusion
qui lui-même est tiré du repo de XavierXao: https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

Mes objectifs ici sont les suivant:
- Traduire les instructions en français afin de rendre le modèle accessible aux non-anglophones.
- Simplifier le notebook pour le rendre plus intuitif et enlever les parties non indispensables.

Pour consulter le code original dans son intégralité,

Je précise que pour ne pas dénaturer son propos, et puisque c'est un message personnel de JoePenna qui n'a pas véritablement d'intérêt pour l'utilisation du modèle, j'ai préféré laisser son introduction telle quelle.

Si tu apprécies cette contribution ou que tu veux en savoir plus sur DreamBooth, n'hésite pas à me suivre sur Twitter ou à mettre une étoile à ce repo.

## <a name="notes-by-joe-penna"></a>  Notes de Joe Penna
### **INTRODUCTIONS!**
Hi! My name is Joe Penna.

You might have seen a few YouTube videos of mine under *MysteryGuitarMan*. I'm now a feature film director. You might have seen [ARCTIC](https://www.youtube.com/watch?v=N5aD9ppoQIo&t=6s) or [STOWAWAY](https://www.youtube.com/watch?v=A_apvQkWsVY).

For my movies, I need to be able to train specific actors, props, locations, etc. So, I did a bunch of changes to @XavierXiao's repo in order to train people's faces.

I can't release all the tests for the movie I'm working on, but when I test with my own face, I release those on my Twitter page - [@MysteryGuitarM](https://twitter.com/MysteryGuitarM).

Lots of these tests were done with a buddy of mine -- Niko from CorridorDigital. It might be how you found this repo!

I'm not really a coder. I'm just stubborn, and I'm not afraid of googling. So, eventually, some really smart folks joined in and have been contributing. In this repo, specifically: @djbielejeski @gammagec @MrSaad –– but so many others in our Discord!

This is no longer my repo. This is the people-who-wanna-see-Dreambooth-on-SD-working-well's repo!

Now, if you wanna try to do this... please read the warnings below first:

### **ATTENTION!**
- **C'est une technologie à la pointe**... il n'y a pour le moment pas de manière simple de la faire tourner. Ce repo est tiré d'un repo tiré d'un autre repo.
  - Pour le moment, cela demande beaucoup d'efforts de créer quelque chose qui globalement se rapproche plutôt du rafistolage -- mais qui finit par marcher SUPER bien.
  - Entrez, s'il vous plaît! Ne vous laissez pas intimider -- mais sachez que vous pataugez de nuit à travers la jungle, sans torche...

- Décongeler le modèle demande beaucoup de puissance.
  - Il est désormais possible de faire tourner cela sur un GPU avec 24GB de VRAM (e.g. 3090). L'entrainement sera plus lent, et il faudra vous assurer que ce soit le *seul* programme qui tourne.
  - Si, comme moi, vous n'en possédez pas, Joe Penna a inclus un Jupyter notebook pour vous aider à le faire tourner sur une plateforme de cloud computing. 
  - C'est actuellement adapté pour [runpod.io](https://runpod.io?ref=n8yfwyum), mais cela fonctionne également sur [vast.ai](#vast-ai-setup) / etc.
  
- Cette implémentation n'implémente pas complètement l'idée de Google sur la façon de préserver l'espace latent.

  - La plupart des images similaires à celles que vous utilisez pour l'entrainement seront biaisées par ce dernier.
  - Par exemple, si vous réalisez l'entrainement en utilisant des images d'une personne, toutes les personnes ressembleront à cette dernière. Si vous entrainez avec un objet, tous les objets de ce type ressembleront au votre.

- Il apparaît qu'il n'y ait pas de manière simple d'entrainer deux sujets consécutifs. Vous obtiendrez un fichier de `11-12GB` avant l'étape de pruning.
  - Le notebook fourni a un pruner qui le réduit à environ `~2gb`.
  
- Le mieux est de changer le nom du token par celui d'une célébrité. Voici [la femme de Joe Penna, en utilisant les même paramètres mais pas le même token](#using-the-generated-model)


# <a name="setup"></a> Installation
## <a name="easy-runpod-instructions"></a> Instructions d'utilisation avec RunPod
- Inscrivez-vous sur RunPod. N'hésitez pas à utiliser [mon lien](https://runpod.io?ref=9zat4jx4) ou [celui de Joe Penna](https://runpod.io?ref=n8yfwyum).
- Cliquez sur **Deploy** dans la catégorie `SECURE CLOUD` ou `COMMUNITY CLOUD` sur la gauche de l'interface.
- Suivez les instructions énoncées dans ce [thread](https://twitter.com/Desp_OnChain/status/1577609784441933824?s=20&t=KLrYD4GGif2evwKaJYpeLg)

## <a name="vast-ai-setup"></a>  Instructions pour Vast.AI
- Inscrivez-vous sur [Vast.AI](https://vast.ai/)
- Ajoutez des fonds (Joe suggère de commencer par $10)
- Allez à : [Client - Create page](https://vast.ai/console/create/)
  - Selectionnez pytorch/pytorch comme image docker, ainsi que les bouttons "Use Jupyter Lab Interface" et "Jupyter direct HTTPS"
  - ![img.png](readme-images/vast-ai-step1-select-docker-image.png)
- Il va falloir augmenter votre espace disque, et filtrer selon la RAM GPU (12gb checkpoint files + 4gb model file + regularization images + other stuff adds up fast)
  - Joe suggère d'allouer 150GB
  - ![img.png](readme-images/vast-ai-step2-instance-filters.png)
  - Il est également conseillé de vérifier la vitesse d'Upload/Download pour ne pas gâcher tous vos fonds à attendre que vos téléchargements se terminent.
- Sélectionnez l'instance désirée, et cliquez sur `Rent`, puis allez à la page [Instances](https://vast.ai/console/instances/) et cliquez sur `Open`
  - ![img.png](readme-images/vast-ai-step3-instances.png)
  - Un avertissement de certificat apparaîtra. Ignorez-le ou installez le [certificat Vast](https://vast.ai/static/jvastai_root.cer).
- Cliquez sur `Notebook -> Python 3` (Il y a plusieurs façons de faire mais c'est ce que Joe recommande).
  - ![img.png](readme-images/vast-ai-step4-get-repo.png)
- Clonez le repo en utilisant cette commande:
  - `!git clone https://github.com/Desp-OnChain/Dreambooth-Stable-Diffusion-FR.git`
  - Cliquez sur `run`
  - ![img.png](readme-images/vast-ai-step5-clone-repo.png)
- Déplacez-vous dans le dossier `Dreambooth-Stable-Diffusion` sur la gauche de l'interface et ouvrez le fichier `dreambooth_runpod_joepenna.ipynb`
  - ![img.png](readme-images/vast-ai-step6-open-notebook.png)
- Suivez les instructions du notebook et commencez l'entrainement.

# <a name="text-vs-dreamb"></a>  Textual Inversion vs. Dreambooth
La majorité du code contenu dans ce repo a été réalisé par Rinon Gal et. al, les auteurs du papier de recherche du modèle Textual Inversion.

Quelques morceaux sur les images de régularisation ont été ajoutés car ils considéraient (les collaborateurs du repo de Joe) qu'ils étaient très importants -- tous les chercheurs les ont inclus!

...jusqu'à ce que ses images soient entrainées dans la classe "dog":
<br><img src="https://media.discordapp.net/attachments/1024716296610385981/1024933960083587102/unknown.png" width="200">

...ou dans la classe "§¶•" qui n'a pas de sens, au lieu de "man" ou "woman" ou "person":
<br><img src="https://media.discordapp.net/attachments/1024716296610385981/1024934146415529984/unknown.png" width="200">

...et avec des images de régularisation complètement vides:
<br><img src="https://media.discordapp.net/attachments/1023293330601287711/1024933371102629898/IMG_7579.JPG" width="200">

Et voilà à quoi `"photograph of an apple"` ressemblait before avant que Joe touche un peu au code:
<br><img src="https://media.discordapp.net/attachments/1018943815370952855/1018946569850069052/unknown.png" width="200">

Ils ne se rendent pas compte que les morceaux "regularization class" de ce code n'ont pas d'effet, et qu'il y a peu ou pas du tout de preservation loss.

Donc, par respect pour le MIT et les chercheurs de chez Google, il a renommé ce fork:
*"Unfrozen Model Textual Inversion for Stable Diffusion"*.

Pour une autre implémentation qui essaye de préserver la prior loss, voir ["Alternative"](#hugging-face-diffusers) below.


# <a name="using-the-generated-model"></a> Utiliser le modèle généré
La `ground truth` (image réelle)
<br><img src="https://user-images.githubusercontent.com/100188076/192403948-8d1d0e50-3e9f-495f-b8ba-1bcb6b536fc8.png" width="200">

Même entrée pour toutes les images ci-dessous:

| `sks` | `woman` | `Natalie Portman` | `Kate Mara` |
| ----- | ------- | ----------------- | ----------- |
| <img src="https://user-images.githubusercontent.com/100188076/192403506-ab96c652-f7d0-47b0-98fa-267defa1e511.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403491-cb258777-5091-4492-a6cc-82305fa729f4.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403437-f9a93720-d41c-4334-8901-fa2d2a10fe36.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403461-1f6972d9-64d0-46b0-b2ed-737e47aae31e.png" width="200"> |   

# <a name="debugging-your-results"></a> Debugger vos résultats
### ❗❗ L'ERREUR NUMERO 1 ❗❗

**Donner seulement votre token en entrée, sans la classe. Par exemple "joepenna" au lieu de "joepenna person"**


Si vous avez entrainé le modèle avec `joepenna` dans la classe `person`, le modèle devrait seulement connaître votre visage sous la description suivante:

```
joepenna person
```

Exemples d'entrées:

🚫 Incorrect (il manque `person` après `joepenna`)
```
portrait photograph of joepenna 35mm film vintage glass
```

✅ Correct (il y a bien `person` après `joepenna`)
```
portrait photograph of joepenna person 35mm film vintage glass
```

Il est possible que parfois vous obteniez quelqu'un qui ressemble à vous avec joepenna (surtout si vous avez entrainé sur trop d'étapes), mais c'est uniquement parce que cette itération de Dreambooth surentraine ce token à tel point qu'il déborde sur l'autre.

---

#### ☢ Faites attention au genre d'images que vous utilisez pour l'entrainement

Pendant l'entrainement, le modèle ne sait pas que vous êtes une personne. Il va juste imiter ce qu'il voit.

Donc, si vous entrainez avec ce genre d'images:

![](readme-images/caution-training.png)

Vous allez seulement obtenir des images de vous dehors à coté d'un arbre, avec un t-shirt blanc et gris, dans un style selfie.

Privilégiez plutôt des images de ce genre:

![](readme-images/better-training-images.png)

Le seul point commun entre ces images est le sujet. Donc le modèle va parcourir les images et retenir seulement votre visage, ce qui rendra la génération dans d'autres styles possible.

## Oh non! Vous n'obtenez pas de bonnes générations!

#### <a name="they-dont-look-like-you"></a> OPTION 1: Ils ne vous ressemblent pas du tout! (Entrainez plus longtemps, ou cherchez de meilleures images d'entrainement)

Vous êtes sûr que votre entrée est bonne?

Il faut que ce soit `<token> <class>`, pas juste `<token>`. Par exemple:

`JoePenna person, portrait photograph, 85mm medium format photo`


Si ça ne vous ressemble toujours pas, c'est que vous n'avez pas entrainé suffisamment longtemps.

----

#### <a name="they-sorta-look-like-you-but-exactly-like-your-training-images"></a> OPTION 2: Ils vous ressemblent, mais ils ressemblent tous à vos images d'entrainement. (Entrainez sur moins d'étapes, cherchez de meilleures images d'entrainement, modifiez l'entrée)

Plusieurs raisons possibles: vous avez entrainé trop longtemps... ou vos images se ressemblaient trop... ou vous n'avez pas entrainé avec assez d'images.

Pas de problème. On peut réparer ça avec la description d'entrée. Stable Diffusion accorde énormément d'importance à ce que vous écrivez en premier. Reportez-le donc plus loin dans la phrase:

`an exquisite portrait photograph, 85mm medium format photo of JoePenna person with a classic haircut`


----

#### <a name="they-look-like-you-but-not-when-you-try-different-styles"></a> OPTION 3: Ils vous ressemblent, mais pas quand vous essayez avec d'autres styles. (Entrainez plus longtemps, cherchez de meilleures images d'entrainement)

Vous n'avez probablement pas entrainé assez longtemps...

Pas de problème. On peut réparer ça avec la description d'entrée:

`JoePenna person in a portrait photograph, JoePenna person in a 85mm medium format photo of JoePenna person`


### Plus d'astuces et d'aide ici: [Stable Diffusion Dreambooth Discord](https://discord.com/invite/qbMuXBXyHA)

# <a name="hugging-face-diffusers"></a> Hugging Face Diffusers - Option alternative

Note: C'est une implémentation en diffuser, l'utilisation est plus compliquée qu'avec un fichier *.ckpy.

Pour le moment, il n'y a pas de moyen d'utiliser les modèles diffusers avec la plupart des repos (e.g. AUTOMATIC1111, HLKY, DeForum, etc)

Dreambooth est maintenant supporté par les diffusers Hugging Face pour l'entrainement avec stable diffusion, vous pouvez essayer dans google colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/notebooks/blob/main/diffusers/sd_dreambooth_training.ipynb)
