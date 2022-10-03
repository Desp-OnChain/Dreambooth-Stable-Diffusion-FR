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
- Follow these video instructions here:

[![VIDEO INSTRUCTIONS](https://img.youtube.com/vi/7m__xadX0z0/0.jpg)](https://www.youtube.com/watch?v=7m__xadX0z0#t=5m33.1s)

## <a name="vast-ai-setup"></a>  Vast.AI Instructions
- Sign up for [Vast.AI](https://vast.ai/)
- Add some funds (I typically add them in $10 increments)
- Navigate to the [Client - Create page](https://vast.ai/console/create/)
  - Select pytorch/pytorch as your docker image, and the buttons "Use Jupyter Lab Interface" and "Jupyter direct HTTPS"
  - ![img.png](readme-images/vast-ai-step1-select-docker-image.png)
- You will want to increase your disk space, and filter on GPU RAM (12gb checkpoint files + 4gb model file + regularization images + other stuff adds up fast)
  - I typically allocate 150GB
  - ![img.png](readme-images/vast-ai-step2-instance-filters.png)
  - Also good to check the Upload/Download speed for enough bandwidth so you don't spend all your money waiting for things to download.
- Select the instance you want, and click `Rent`, then head over to your [Instances](https://vast.ai/console/instances/) page and click `Open`
  - ![img.png](readme-images/vast-ai-step3-instances.png)
  - You will get an unsafe certificate warning. Click past the warning or install the [Vast cert](https://vast.ai/static/jvastai_root.cer).
- Click `Notebook -> Python 3` (You can do this next step a number of ways, but I typically do this)
  - ![img.png](readme-images/vast-ai-step4-get-repo.png)
- Clone Joe's repo with this command
  - `!git clone https://github.com/JoePenna/Dreambooth-Stable-Diffusion.git`
  - Click `run`
  - ![img.png](readme-images/vast-ai-step5-clone-repo.png)
- Navigate into the new `Dreambooth-Stable-Diffusion` directory on the left and open the `dreambooth_runpod_joepenna.ipynb` file
  - ![img.png](readme-images/vast-ai-step6-open-notebook.png)
- Follow the instructions in the workbook and start training

# <a name="text-vs-dreamb"></a>  Textual Inversion vs. Dreambooth
The majority of the code in this repo was written by Rinon Gal et. al, the authors of the Textual Inversion research paper.

A few bits about regularization images were added that we all thought were super important -- all the researchers included!

...until my images were trained under the class "dog":
<br><img src="https://media.discordapp.net/attachments/1024716296610385981/1024933960083587102/unknown.png" width="200">

...and under the nonsensical class "§¶•" instead of "man" or "woman" or "person":
<br><img src="https://media.discordapp.net/attachments/1024716296610385981/1024934146415529984/unknown.png" width="200">

...and with completely blank regularization images:
<br><img src="https://media.discordapp.net/attachments/1023293330601287711/1024933371102629898/IMG_7579.JPG" width="200">

And here's what `"photograph of an apple"` looked like before I messed with code a bit:
<br><img src="https://media.discordapp.net/attachments/1018943815370952855/1018946569850069052/unknown.png" width="200">

We're not realizing the "regularization class" bits of this code have no effect, and that there is little to no prior preservation loss.

So, out of respect to both the MIT team and the Google researchers, I'm renaming this fork to:
*"Unfrozen Model Textual Inversion for Stable Diffusion"*.

For an alternate implementation that attempts prior loss preservation, please see ["Alternate Option"](#hugging-face-diffusers) below.


# <a name="using-the-generated-model"></a> Using the generated model
The `ground truth` (real picture, caution: very beautiful woman)
<br><img src="https://user-images.githubusercontent.com/100188076/192403948-8d1d0e50-3e9f-495f-b8ba-1bcb6b536fc8.png" width="200">

Same prompt for all of these images below:

| `sks` | `woman` | `Natalie Portman` | `Kate Mara` |
| ----- | ------- | ----------------- | ----------- |
| <img src="https://user-images.githubusercontent.com/100188076/192403506-ab96c652-f7d0-47b0-98fa-267defa1e511.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403491-cb258777-5091-4492-a6cc-82305fa729f4.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403437-f9a93720-d41c-4334-8901-fa2d2a10fe36.png" width="200"> | <img src="https://user-images.githubusercontent.com/100188076/192403461-1f6972d9-64d0-46b0-b2ed-737e47aae31e.png" width="200"> |   

# <a name="debugging-your-results"></a> Debugging your results
### ❗❗ THE NUMBER ONE MISTAKE PEOPLE MAKE ❗❗

**Prompting with just your token. ie "joepenna" instead of "joepenna person"**


If you trained with `joepenna` under the class `person`, the model should only know your face as:

```
joepenna person
```

Example Prompts:

🚫 Incorrect (missing `person` following `joepenna`)
```
portrait photograph of joepenna 35mm film vintage glass
```

✅ This is right (`person` is included after `joepenna`)
```
portrait photograph of joepenna person 35mm film vintage glass
```

You might sometimes get someone who kinda looks like you with joepenna (especially if you trained for too many steps), but that's only because this current iteration of Dreambooth overtrains that token so much that it bleeds into that token.

---

#### ☢ Be careful with the types of images you train

While training, Stable doesn't know that you're a person. It's just going to mimic what it sees.

So, if these are your training images look like this:

![](readme-images/caution-training.png)

You're only going to get generations of you outside next to a spiky tree, wearing a white-and-gray shirt, in the style of... well, selfie photograph.

Instead, this training set is much better:

![](readme-images/better-training-images.png)

The only thing that is consistent between images is the subject. So, Stable will look through the images and learn only your face, which will make "editing" it into other styles possible.

## Oh no! You're not getting good generations!

#### <a name="they-dont-look-like-you"></a> OPTION 1: They're not looking like you at all! (Train longer, or get better training images)

Are you sure you're prompting it right?

It should be `<token> <class>`, not just `<token>`. For example:

`JoePenna person, portrait photograph, 85mm medium format photo`


If it still doesn't look like you, you didn't train long enough.

----

#### <a name="they-sorta-look-like-you-but-exactly-like-your-training-images"></a> OPTION 2: They're looking like you, but are all looking like your training images. (Train for less steps, get better training images, fix with prompting)

Okay, a few reasons why: you might have trained too long... or your images were too similar... or you didn't train with enough images.

No problem. We can fix that with the prompt. Stable Diffusion puts a LOT of merit to whatever you type first. So save it for later:

`an exquisite portrait photograph, 85mm medium format photo of JoePenna person with a classic haircut`


----

#### <a name="they-look-like-you-but-not-when-you-try-different-styles"></a> OPTION 3: They're looking like you, but not when you try different styles. (Train longer, get better training images)

You didn't train long enough...

No problem. We can fix that with the prompt:

`JoePenna person in a portrait photograph, JoePenna person in a 85mm medium format photo of JoePenna person`


### More tips and help here: [Stable Diffusion Dreambooth Discord](https://discord.com/invite/qbMuXBXyHA)

# <a name="hugging-face-diffusers"></a> Hugging Face Diffusers - Alternate Option

Note: This is a diffuser implementation, and use is much more complicated than using a *.ckpy file.

At the moment, there is no way to use the diffusers model with most repos (e.g. AUTOMATIC1111, HLKY, DeForum, etc)

Dreambooth is now supported in Hugging Face diffusers for training with stable diffusion, try it out in the colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/notebooks/blob/main/diffusers/sd_dreambooth_training.ipynb)
