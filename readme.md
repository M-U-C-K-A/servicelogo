# servicelogo

Une collection de logos façon **sticker kawaii** : langages, frameworks, services… et maintenant les matières de l'Éducation nationale.
Chaque logo suit la même recette : de grosses lettres arrondies, la lecture du nom en japonais, un petit gag de dev (ou d'élève), un contour blanc épais et une ombre décalée.

> [!NOTE]
> Je ne suis pas l'auteur de ce style : les logos d'origine sont de **[SAWARATSUKI](https://github.com/SAWARATSUKI/KawaiiLogos)** (さわらつき). Ce dépôt les reprend et ajoute de nouveaux logos dans le même esprit. Merci à SAWARATSUKI 🙏

<p align="center">
  <img src="images/Docker.png" width="32%" alt="Docker">
  <img src="images/education/Maths.png" width="32%" alt="Maths">
  <img src="images/ClaudeCode.png" width="32%" alt="Claude Code">
</p>

## Sommaire

- [Anatomie d'un logo](#anatomie-dun-logo)
- [Remplissage bicolore](#remplissage-bicolore)
- [Du texte au sticker](#du-texte-au-sticker)
- [Modifier un logo](#modifier-un-logo)
- [Exporter en PNG](#exporter-en-png)
- [Tous les logos](#tous-les-logos)
- [Crédits](#crédits)

## Anatomie d'un logo

<p align="center"><img src="docs/layers.svg" width="860" alt="Les trois calques d'un logo, en vue isométrique éclatée : ombre, contour blanc, contenu"></p>

Un logo, c'est **un seul fichier SVG** avec trois calques. Les formes (lettres, icônes, cartes) sont déclarées une seule fois dans `<defs>` sous forme de silhouette sans couleur, puis réutilisées deux fois avec `<use>` :

```svg
<defs>
  <g id="silhouette"> … les formes, sans fill ni stroke … </g>
</defs>

<!-- 1. Ombre : la silhouette, décalée -->
<use xlink:href="#silhouette" transform="translate(8 34)"
     fill="#0b3a8c" stroke="#0b3a8c" stroke-width="58" stroke-linejoin="round"/>

<!-- 2. Contour blanc : la même silhouette, épaissie -->
<use xlink:href="#silhouette"
     fill="#ffffff" stroke="#ffffff" stroke-width="58" stroke-linejoin="round"/>

<!-- 3. Contenu : lettres, kana, icônes, cartes -->
<g> … </g>
```

| Calque | Rôle | Bon à savoir |
|---|---|---|
| 1. Ombre | l'effet « extrudé » sous le sticker | sa couleur est une version foncée de la couleur principale |
| 2. Contour blanc | le bord découpé du sticker | le trait est centré : `stroke-width="58"` donne 29 px de blanc visible |
| 3. Contenu | tout ce qui est en couleur | les petits textes ont leur propre halo blanc pour rester lisibles quand ils chevauchent les lettres |

## Remplissage bicolore

<p align="center"><img src="docs/two-tone.svg" width="860" alt="Remplissage bicolore en vue isométrique : un fond, une vague, puis la forme des lettres qui découpe le tout"></p>

Les grosses lettres ne sont pas d'une seule couleur : un rectangle de la couleur de base et une vague plus claire sont découpés par la forme des lettres grâce à un `clipPath`.

```svg
<clipPath id="lettres"><use xlink:href="#mot"/></clipPath>

<g clip-path="url(#lettres)">
  <rect width="1920" height="1080" fill="#1d63ed"/>   <!-- 1. fond -->
  <path d="M500,480 L512,476 … Z" fill="#4aa8ff"/>    <!-- 2. vague -->
</g>
```

## Du texte au sticker

<p align="center"><img src="docs/pipeline.svg" width="960" alt="Les quatre étapes : texte vers tracés, silhouette, contour et ombre, recentrage"></p>

1. **Texte → tracés.** Tous les textes sont convertis en `<path>` avec [fontTools](https://github.com/fonttools/fonttools) : les SVG n'ont besoin d'aucune police installée et s'affichent pareil partout. Polices utilisées : SF Pro Rounded Black (lettres), Hiragino Maru Gothic (kana et kanji), JetBrains Mono (code).
2. **Silhouette sans trous.** Pour le contour, on ne garde que le contour extérieur de chaque lettre. Pour les lettres rondes (o, e, c, a, s…), on prend même leur enveloppe convexe. Sans ça, l'ombre se verrait à travers le trou du « o ».
3. **Contour et ombre.** Le `stroke` arrondi épaissit la silhouette comme une dilatation ; une copie décalée en dessous fait l'ombre.
4. **Recentrage.** Chaque SVG est rendu une fois dans Chrome, sa boîte englobante est mesurée, puis tout est recentré (et réduit si besoin) dans le canevas 1920×1080 avec un `<g transform="translate(…) scale(…)">`.

## Modifier un logo

| Je veux changer… | Où regarder |
|---|---|
| la couleur de l'ombre | le premier `<use xlink:href="#silhouette">` après `</defs>` (`fill` et `stroke`) |
| l'épaisseur du contour | le `stroke-width` des deux `<use>` (58 par défaut) |
| les couleurs des lettres | le `<rect>` (fond) et le `<path>` (vague) du groupe `clip-path` |
| la taille ou la position globale | le `<g transform="translate(…) scale(…)">` qui enveloppe tout |

Les textes étant des tracés, changer un mot demande de régénérer le logo ou de le retoucher dans un éditeur vectoriel (Figma, Inkscape…).

## Exporter en PNG

Chaque logo existe en PNG dans `images/` (1920×1080, fond transparent) ; les sources vectorielles des nouveaux logos sont dans `svg/`. Après avoir modifié un SVG, on le réexporte avec Chrome (c'est ce qui a servi pour tous les PNG du dépôt) :

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --hide-scrollbars --window-size=1920,1080 --default-background-color=00000000 --screenshot="$PWD/images/Docker.png" "file://$PWD/svg/Docker.svg"
```

N'importe quel outil qui lit le SVG fait aussi l'affaire (Inkscape, rsvg-convert, Figma…).

## Tous les logos

### Langages, frameworks et outils

*Les logos d'origine, par [SAWARATSUKI](https://github.com/SAWARATSUKI/KawaiiLogos).*

| | | |
|:---:|:---:|:---:|
| <a href="images/Angular.png"><img src="images/Angular.png" width="260" alt="Angular"></a><br><sub><b>Angular</b></sub> | <a href="images/AngularNewLogo.png"><img src="images/AngularNewLogo.png" width="260" alt="Angular (nouveau logo)"></a><br><sub><b>Angular (nouveau logo)</b></sub> | <a href="images/Arch%20Linux.png"><img src="images/Arch%20Linux.png" width="260" alt="Arch Linux"></a><br><sub><b>Arch Linux</b></sub> |
| <a href="images/Astro.png"><img src="images/Astro.png" width="260" alt="Astro"></a><br><sub><b>Astro</b></sub> | <a href="images/C%2B%2B.png"><img src="images/C%2B%2B.png" width="260" alt="C++"></a><br><sub><b>C++</b></sub> | <a href="images/C.png"><img src="images/C.png" width="260" alt="C"></a><br><sub><b>C</b></sub> |
| <a href="images/Clion.png"><img src="images/Clion.png" width="260" alt="CLion"></a><br><sub><b>CLion</b></sub> | <a href="images/Cloudflare.png"><img src="images/Cloudflare.png" width="260" alt="Cloudflare"></a><br><sub><b>Cloudflare</b></sub> | <a href="images/COBOL.png"><img src="images/COBOL.png" width="260" alt="COBOL"></a><br><sub><b>COBOL</b></sub> |
| <a href="images/Croud.png"><img src="images/Croud.png" width="260" alt="Croud"></a><br><sub><b>Croud</b></sub> | <a href="images/CSS.png"><img src="images/CSS.png" width="260" alt="CSS"></a><br><sub><b>CSS</b></sub> | <a href="images/Figma.png"><img src="images/Figma.png" width="260" alt="Figma"></a><br><sub><b>Figma</b></sub> |
| <a href="images/FlutterTransparent.png"><img src="images/FlutterTransparent.png" width="260" alt="Flutter"></a><br><sub><b>Flutter</b></sub> | <a href="images/Github.png"><img src="images/Github.png" width="260" alt="GitHub"></a><br><sub><b>GitHub</b></sub> | <a href="images/Gitlab.png"><img src="images/Gitlab.png" width="260" alt="GitLab"></a><br><sub><b>GitLab</b></sub> |
| <a href="images/Golang.png"><img src="images/Golang.png" width="260" alt="Go"></a><br><sub><b>Go</b></sub> | <a href="images/Haskell.png"><img src="images/Haskell.png" width="260" alt="Haskell"></a><br><sub><b>Haskell</b></sub> | <a href="images/Hono.png"><img src="images/Hono.png" width="260" alt="Hono"></a><br><sub><b>Hono</b></sub> |
| <a href="images/HTML.png"><img src="images/HTML.png" width="260" alt="HTML"></a><br><sub><b>HTML</b></sub> | <a href="images/htmx.png"><img src="images/htmx.png" width="260" alt="htmx"></a><br><sub><b>htmx</b></sub> | <a href="images/JavaTransparent.png"><img src="images/JavaTransparent.png" width="260" alt="Java"></a><br><sub><b>Java</b></sub> |
| <a href="images/JUNIPERTRANS.png"><img src="images/JUNIPERTRANS.png" width="260" alt="Juniper"></a><br><sub><b>Juniper</b></sub> | <a href="images/Kotlin.png"><img src="images/Kotlin.png" width="260" alt="Kotlin"></a><br><sub><b>Kotlin</b></sub> | <a href="images/LaravelTransparent.png"><img src="images/LaravelTransparent.png" width="260" alt="Laravel"></a><br><sub><b>Laravel</b></sub> |
| <a href="images/MUIT.png"><img src="images/MUIT.png" width="260" alt="MUI"></a><br><sub><b>MUI</b></sub> | <a href="images/Next.js.png"><img src="images/Next.js.png" width="260" alt="Next.js"></a><br><sub><b>Next.js</b></sub> | <a href="images/Node.js.png"><img src="images/Node.js.png" width="260" alt="Node.js"></a><br><sub><b>Node.js</b></sub> |
| <a href="images/Photoshop.png"><img src="images/Photoshop.png" width="260" alt="Photoshop"></a><br><sub><b>Photoshop</b></sub> | <a href="images/Python.png"><img src="images/Python.png" width="260" alt="Python"></a><br><sub><b>Python</b></sub> | <a href="images/Qwik.png"><img src="images/Qwik.png" width="260" alt="Qwik"></a><br><sub><b>Qwik</b></sub> |
| <a href="images/Raspberry%20Pi.png"><img src="images/Raspberry%20Pi.png" width="260" alt="Raspberry Pi"></a><br><sub><b>Raspberry Pi</b></sub> | <a href="images/React.png"><img src="images/React.png" width="260" alt="React"></a><br><sub><b>React</b></sub> | <a href="images/RhineLab.png"><img src="images/RhineLab.png" width="260" alt="RhineLab"></a><br><sub><b>RhineLab</b></sub> |
| <a href="images/Rider.png"><img src="images/Rider.png" width="260" alt="Rider"></a><br><sub><b>Rider</b></sub> | <a href="images/RStudioTransparent.png"><img src="images/RStudioTransparent.png" width="260" alt="RStudio"></a><br><sub><b>RStudio</b></sub> | <a href="images/Ruby.png"><img src="images/Ruby.png" width="260" alt="Ruby"></a><br><sub><b>Ruby</b></sub> |
| <a href="images/Rust.png"><img src="images/Rust.png" width="260" alt="Rust"></a><br><sub><b>Rust</b></sub> | <a href="images/Streamloots.png"><img src="images/Streamloots.png" width="260" alt="Streamloots"></a><br><sub><b>Streamloots</b></sub> | <a href="images/SwiftTransparent.png"><img src="images/SwiftTransparent.png" width="260" alt="Swift"></a><br><sub><b>Swift</b></sub> |
| <a href="images/Tailwindcss6.png"><img src="images/Tailwindcss6.png" width="260" alt="Tailwind CSS"></a><br><sub><b>Tailwind CSS</b></sub> | <a href="images/TeamSpeak.png"><img src="images/TeamSpeak.png" width="260" alt="TeamSpeak"></a><br><sub><b>TeamSpeak</b></sub> | <a href="images/TransparentGNU.png"><img src="images/TransparentGNU.png" width="260" alt="GNU"></a><br><sub><b>GNU</b></sub> |
| <a href="images/TypeScript.png"><img src="images/TypeScript.png" width="260" alt="TypeScript"></a><br><sub><b>TypeScript</b></sub> | <a href="images/UnityBlenderT.png"><img src="images/UnityBlenderT.png" width="260" alt="Unity / Blender"></a><br><sub><b>Unity / Blender</b></sub> | <a href="images/VIMTRANS.png"><img src="images/VIMTRANS.png" width="260" alt="Vim"></a><br><sub><b>Vim</b></sub> |
| <a href="images/Vite.png"><img src="images/Vite.png" width="260" alt="Vite"></a><br><sub><b>Vite</b></sub> | <a href="images/VoiceMod.png"><img src="images/VoiceMod.png" width="260" alt="VoiceMod"></a><br><sub><b>VoiceMod</b></sub> | <a href="images/VRChatTransparent.png"><img src="images/VRChatTransparent.png" width="260" alt="VRChat"></a><br><sub><b>VRChat</b></sub> |
| <a href="images/Vue.png"><img src="images/Vue.png" width="260" alt="Vue"></a><br><sub><b>Vue</b></sub> | <a href="images/WALLHACK.png"><img src="images/WALLHACK.png" width="260" alt="WALLHACK"></a><br><sub><b>WALLHACK</b></sub> | <a href="images/X.png"><img src="images/X.png" width="260" alt="X"></a><br><sub><b>X</b></sub> |

### Dev et services

| | | |
|:---:|:---:|:---:|
| <a href="images/Bun.png"><img src="images/Bun.png" width="260" alt="Bun"></a><br><sub><b>Bun</b> · <a href="svg/Bun.svg">svg</a></sub> | <a href="images/ClaudeCode.png"><img src="images/ClaudeCode.png" width="260" alt="Claude Code"></a><br><sub><b>Claude Code</b> · <a href="svg/ClaudeCode.svg">svg</a></sub> | <a href="images/Cursor.png"><img src="images/Cursor.png" width="260" alt="Cursor"></a><br><sub><b>Cursor</b> · <a href="svg/Cursor.svg">svg</a></sub> |
| <a href="images/Deno.png"><img src="images/Deno.png" width="260" alt="Deno"></a><br><sub><b>Deno</b> · <a href="svg/Deno.svg">svg</a></sub> | <a href="images/Discord.png"><img src="images/Discord.png" width="260" alt="Discord"></a><br><sub><b>Discord</b> · <a href="svg/Discord.svg">svg</a></sub> | <a href="images/Docker.png"><img src="images/Docker.png" width="260" alt="Docker"></a><br><sub><b>Docker</b> · <a href="svg/Docker.svg">svg</a></sub> |
| <a href="images/Firebase.png"><img src="images/Firebase.png" width="260" alt="Firebase"></a><br><sub><b>Firebase</b> · <a href="svg/Firebase.svg">svg</a></sub> | <a href="images/Kubernetes.png"><img src="images/Kubernetes.png" width="260" alt="Kubernetes"></a><br><sub><b>Kubernetes</b> · <a href="svg/Kubernetes.svg">svg</a></sub> | <a href="images/Linux.png"><img src="images/Linux.png" width="260" alt="Linux"></a><br><sub><b>Linux</b> · <a href="svg/Linux.svg">svg</a></sub> |
| <a href="images/macOS.png"><img src="images/macOS.png" width="260" alt="macOS"></a><br><sub><b>macOS</b> · <a href="svg/macOS.svg">svg</a></sub> | <a href="images/NeonDB.png"><img src="images/NeonDB.png" width="260" alt="Neon"></a><br><sub><b>Neon</b> · <a href="svg/NeonDB.svg">svg</a></sub> | <a href="images/Notion.png"><img src="images/Notion.png" width="260" alt="Notion"></a><br><sub><b>Notion</b> · <a href="svg/Notion.svg">svg</a></sub> |
| <a href="images/Obsidian.png"><img src="images/Obsidian.png" width="260" alt="Obsidian"></a><br><sub><b>Obsidian</b> · <a href="svg/Obsidian.svg">svg</a></sub> | <a href="images/Polar.png"><img src="images/Polar.png" width="260" alt="Polar.sh"></a><br><sub><b>Polar.sh</b> · <a href="svg/Polar.svg">svg</a></sub> | <a href="images/Prisma.png"><img src="images/Prisma.png" width="260" alt="Prisma"></a><br><sub><b>Prisma</b> · <a href="svg/Prisma.svg">svg</a></sub> |
| <a href="images/shadcnui.png"><img src="images/shadcnui.png" width="260" alt="shadcn/ui"></a><br><sub><b>shadcn/ui</b> · <a href="svg/shadcnui.svg">svg</a></sub> | <a href="images/Stripe.png"><img src="images/Stripe.png" width="260" alt="Stripe"></a><br><sub><b>Stripe</b> · <a href="svg/Stripe.svg">svg</a></sub> | <a href="images/Supabase.png"><img src="images/Supabase.png" width="260" alt="Supabase"></a><br><sub><b>Supabase</b> · <a href="svg/Supabase.svg">svg</a></sub> |
| <a href="images/Svelte.png"><img src="images/Svelte.png" width="260" alt="Svelte"></a><br><sub><b>Svelte</b> · <a href="svg/Svelte.svg">svg</a></sub> | <a href="images/Vercel.png"><img src="images/Vercel.png" width="260" alt="Vercel"></a><br><sub><b>Vercel</b> · <a href="svg/Vercel.svg">svg</a></sub> | <a href="images/Windows.png"><img src="images/Windows.png" width="260" alt="Windows"></a><br><sub><b>Windows</b> · <a href="svg/Windows.svg">svg</a></sub> |

### Éducation nationale

| Matière | Lecture | Le gag |
|---|---|---|
| Français | ふらんすご *(furansugo)* | « accord du participe passé » → 例外ばっかり！ *« que des exceptions ! »* |
| Maths | すうがく *(sūgaku)* | x² + y² = r² → これ、いつ使うの？ *« on s'en servira quand ? »* |
| Histoire | れきし *(rekishi)* + tampon 歴史 | 1515 Marignan → それしか覚えてない… *« je ne retiens que ça… »* |
| Géographie | ちり *(chiri)* | « Vous êtes ici » → 北はどっち？ *« le nord, c'est où ? »* |
| Anglais | えいご *(eigo)* | Brian is in the kitchen. → ブライアンは台所にいる |
| Physique-Chimie | ぶつり・かがく *(butsuri, kagaku)* | E = mc² → 爆発注意！ *« attention, ça explose ! »* |
| SVT | せいぶつ・ちがく *(seibutsu, chigaku)* | Mitochondrie → 細胞の発電所！ *« la centrale électrique de la cellule ! »* |
| Technologie | ぎじゅつ *(gijutsu)* | blocs Scratch → またScratch？ *« encore Scratch ? »* |
| Philosophie | てつがく *(tetsugaku)* | « Je pense, donc je suis. » → 我思う、ゆえに我あり |
| Sport (EPS) | たいいく *(taiiku)* | BIP ! Palier 7 → もう走れない… *« je peux plus courir… »* |
| Latin | らてんご *(ratengo)* | rosa, rosa, rosam… → 格変化、多すぎ！ *« trop de déclinaisons ! »* |
| Grec | ぎりしゃご *(girishago)* | ΓΝΩΘΙ ΣΕΑΥΤΟΝ → 全然読めない… *« j'y comprends rien… »* |
| Musique | おんがく *(ongaku)* | Flûte à bec → ピーッ！音が外れた… *« piiih ! fausse note… »* |
| Arts plastiques | びじゅつ *(bijutsu)* | Sujet : « libre » → 自由が一番むずかしい *« le sujet libre, c'est le plus dur »* |
| SES | けいざい・しゃかい *(keizai, shakai)* | Offre = Demande → 需要と供給！ *« l'offre et la demande ! »* |

| | | |
|:---:|:---:|:---:|
| <a href="images/education/Francais.png"><img src="images/education/Francais.png" width="260" alt="Français"></a><br><sub><b>Français</b> · <a href="svg/education/Francais.svg">svg</a></sub> | <a href="images/education/Maths.png"><img src="images/education/Maths.png" width="260" alt="Maths"></a><br><sub><b>Maths</b> · <a href="svg/education/Maths.svg">svg</a></sub> | <a href="images/education/Histoire.png"><img src="images/education/Histoire.png" width="260" alt="Histoire"></a><br><sub><b>Histoire</b> · <a href="svg/education/Histoire.svg">svg</a></sub> |
| <a href="images/education/Geographie.png"><img src="images/education/Geographie.png" width="260" alt="Géographie"></a><br><sub><b>Géographie</b> · <a href="svg/education/Geographie.svg">svg</a></sub> | <a href="images/education/Anglais.png"><img src="images/education/Anglais.png" width="260" alt="Anglais"></a><br><sub><b>Anglais</b> · <a href="svg/education/Anglais.svg">svg</a></sub> | <a href="images/education/PhysiqueChimie.png"><img src="images/education/PhysiqueChimie.png" width="260" alt="Physique-Chimie"></a><br><sub><b>Physique-Chimie</b> · <a href="svg/education/PhysiqueChimie.svg">svg</a></sub> |
| <a href="images/education/SVT.png"><img src="images/education/SVT.png" width="260" alt="SVT"></a><br><sub><b>SVT</b> · <a href="svg/education/SVT.svg">svg</a></sub> | <a href="images/education/Techno.png"><img src="images/education/Techno.png" width="260" alt="Technologie"></a><br><sub><b>Technologie</b> · <a href="svg/education/Techno.svg">svg</a></sub> | <a href="images/education/Philosophie.png"><img src="images/education/Philosophie.png" width="260" alt="Philosophie"></a><br><sub><b>Philosophie</b> · <a href="svg/education/Philosophie.svg">svg</a></sub> |
| <a href="images/education/Sport.png"><img src="images/education/Sport.png" width="260" alt="Sport (EPS)"></a><br><sub><b>Sport (EPS)</b> · <a href="svg/education/Sport.svg">svg</a></sub> | <a href="images/education/Latin.png"><img src="images/education/Latin.png" width="260" alt="Latin"></a><br><sub><b>Latin</b> · <a href="svg/education/Latin.svg">svg</a></sub> | <a href="images/education/Grec.png"><img src="images/education/Grec.png" width="260" alt="Grec"></a><br><sub><b>Grec</b> · <a href="svg/education/Grec.svg">svg</a></sub> |
| <a href="images/education/Musique.png"><img src="images/education/Musique.png" width="260" alt="Musique"></a><br><sub><b>Musique</b> · <a href="svg/education/Musique.svg">svg</a></sub> | <a href="images/education/ArtsPlastiques.png"><img src="images/education/ArtsPlastiques.png" width="260" alt="Arts plastiques"></a><br><sub><b>Arts plastiques</b> · <a href="svg/education/ArtsPlastiques.svg">svg</a></sub> | <a href="images/education/SES.png"><img src="images/education/SES.png" width="260" alt="SES"></a><br><sub><b>SES</b> · <a href="svg/education/SES.svg">svg</a></sub> |

## Crédits

- Les logos « Langages, frameworks et outils » sont de **SAWARATSUKI**. 画像はさわらつきが提供しています。
- Les logos « Dev et services » et « Éducation nationale » (PNG dans `images/`, sources dans `svg/`) ainsi que les schémas (`docs/`) ont été créés pour ce dépôt en reprenant ce style.
- Les marques et logos cités appartiennent à leurs propriétaires respectifs : ce sont des fan-arts non officiels.
