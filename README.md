# Distribution Starter Kit

**Attention : Ce profile n'installe aucun des modules qu'il contient. Seuls les modules de base du coeur et certains modules contrib sont installés par défaut.**

Modules installés par défaut :  
* `block`
* `config`
* `config_split`
* `dynamic_page_cache`
* `help`
* `language`
* `page_cache`
* `redis`
* `toolbar`
* `user`

Pour activer tous les modules de cette distribution :

```
$ drush en sk_commons sk_media_file sk_media_image sk_media_video_embed sk_media_video_file sk_node_page sk_paragraph_media_image_video sk_paragraph_slider sk_paragraph_text_image sk_paragraph_title_text -y
```

## Les modules de cette distribution contiennent

* La définition de `field.storage` de base (`sk_commons`)
* La définition de types de média (`sk_media_*`)
* La définition de types de paragraphes (`sk_paragraph_*`)
* La définition de types de contenu (`sk_node_*`)
* Les `entity_browser` pour chaque type d'entité (`sk_commons`)
* La configuration de `paragraphs_browser` pour faciliter la contribution (`sk_commons`)

## Les modules disponibles

|Module|Description|
|---|---|
|[sk_commons](#sk_commons)|Configurations communes à tous les modules de la distribution.|
|[sk_media_file](#sk_media_file)|Défini un type de media fichier à uploader.|
|[sk_media_image](#sk_media_image)|Défini un type de media image à uploader.|
|[sk_media_video_embed](#sk_media_video_embed)|Défini un type de media contenant une vidéo générée à partir d'une URL (ex. Youtube).|
|[sk_media_video_file](#sk_media_video_file)|Défini un type de média contenant une vidéo à uploader.|
|[sk_node_page](#sk_node_page)|Défini un Type de contenu page permettant de référencer tous les paragraphes de la distribution.|
|[sk_paragraph_media_image_video](#sk_paragraph_media_image_video)|Défini un type de paragraphe pour référencer une image ou une vidéo (embed ou file).|
|[sk_paragraph_slider](#sk_paragraph_slider)|Défini un type de paragraphe pour créer un slider.|
|[sk_paragraph_text_image](#sk_paragraph_text_image)|Défini un type de paragraphe pour référencer une image et du texte.|
|[sk_paragraph_title_text](#sk_paragraph_title_text)|Défini un type de paragraphe avec un titre et du texte.|

## sk_commons

### Dépendances

Aucune

#### Field storage

Le module fourni tous les `field.storage` utilisés par les autres modules
La définition des `field.storage` est optionnelle. Chaque `field.storage` ne sera créé que lorsqu'au moins un des modules l'utilisant sera activé.

|Nom machine|Type|Description|
|---|---|---|
|`media.field_file`|`file`|Upload d'un fichier dans un media|
|`media.field_image`|`image`|Upload d'une image dans un media|
|`media.field_video_embed`|`video_embed_field`|Lien vers une vidéo dans un media|
|`node.field_paragraphs`|`entity_reference_revisions`|Entity reference vers un ou plusieurs paragraphes dans un contenu|
|`paragraph.field_image_position`|`list_string`|Liste de choix avec les valeurs `Left` / `Center` / `Right` dans un paragraphe|
|`paragraph.field_media`|`entity_reference`|Entity reference vers un media dans paragraphe|
|`paragraph.field_node`|`entity_reference`|Entity reference vers un node dans un paragraphe|
|`paragraph.field_paragraphs`|`entity_reference_revisions`|Entity reference vers un ou plusieurs paragraphe|
|`paragraph.field_subtitle`|`text_long`|Sous-titre dans un paragraphe|
|`paragraph.field_text`|`text_long`|Texte dans un paragraphe|
|`paragraph.field_title`|`string (255)`|Titre dans un paragraphe|

#### Entity browser

L'entity browser présent dans ce module permet de naviguer dans tous les contenus de type `media`.

La vue associée pour effectuer la recherche se base sur les valeur du paramètre `target_bundle` du champ depuis lequel l'entity browser est appelé.

**Exemple :** Si un champ à comme configuration :
```
settings:  
  handler_settings:
    target_bundles:
      image: image  
      video_embed: video_embed  
      video_file: video_file
```
Alors l'entity browser n'affichera que les media de type `image`, `video_file`et `video_embed`.  
Cette opération est possible grâce au patch de l'issue https://www.drupal.org/project/entity_browser/issues/2865928

Un onglet de création est défini pour chaque type de media présent dans les modules de la distribution.  
Les onglets sont eux aussi affichés en fonction des paramètres `target_bundle` du champ depuis lequel l'entity browser est appelé (c.f. `sk_commons_form_alter()`).

## sk_media_file

### Dépendances

* `drupal:file`
* `drupal:image`
* `drupal:media`
* `starter_kit:sk_commons`

#### Type de contenu

**Entity type :** `media`  
**Bundle :** `file`  
**Label :** File  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|File|`media.file.field_file`|`media.field_file`||

## sk_media_image

### Dépendances

* `drupal:image`
* `drupal:media`
* `starter_kit:sk_commons`

#### Type de contenu

**Entity type :** `media`  
**Bundle :** `image`  
**Label :** Image  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Image|`media.image.field_image`|`media.field_image`|Tout type de fichier image uniquement.|

## sk_media_video_embed

### Dépendances

* `drupal:media`
* `video_embed_field:video_embed_media`
* `starter_kit:sk_commons`

#### Type de contenu

**Entity type :** `media`  
**Bundle :** `video_embed`  
**Label :** Video Embed  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Video Url|`media.video_embed.field_video_embed`|`media.field_video_embed`|Vidéo Youtube + Vimeo uniquement.|

## sk_media_video_file

### Dépendances

* `drupal:file`
* `drupal:media`
* `starter_kit:sk_commons`

#### Type de contenu

**Entity type :** `media`  
**Bundle :** `video_file`  
**Label :** Video file  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Video file|`media.video_file.field_file`|`media.field_file`|Vidéo mp4 uniquement.|

## sk_node_page

### Dépendances

* `drupal:node`
* `starter_kit:sk_commons`
* `paragraphs_browser:paragraphs_browser`

#### Type de contenu

**Entity type :** `node`  
**Bundle :** `page`  
**Label :** Page  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Paragraphs|`node.page.field_paragraphs`|`node.field_paragraphs`|Référence les paragraphes de type `media_image_video`, `slider`, `image_text` et `title_text`.|

## sk_paragraph_media_image_video

### Dépendances

* `entity_browser:entity_browser`
* `entity_browser_entity_form:entity_browser_entity_form`
* `starter_kit:sk_commons`
* `paragraphs:paragraphs`
* `drupal:views`

#### Type de contenu

**Entity type :** `paragraph`  
**Bundle :** `media_image_video`  
**Label :** Image or Video  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Media image video|`paragraph.media_image_video.field_media`|`paragraph.field_media`|Référence les media de type `image`, `video_embed` et `video_file`.|

## sk_paragraph_slider

### Dépendances

* `drupal:text`
* `entity_browser:entity_browser`
* `entity_browser_entity_form:entity_browser_entity_form`
* `starter_kit:sk_commons`
* `starter_kit:sk_media_image`
* `paragraphs:paragraphs`
* `drupal:views`

#### Type de contenu

**Entity type :** `paragraph`  
**Bundle :** `slider`  
**Label :** Slider  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Title|`paragraph.slider.field_title`|`paragraph.field_title`||
|Slide|`paragraph.slider.field_paragraphs`|`paragraph.field_paragraphs`|Référence les paragraphes de type `slide` uniquement.|

**Entity type :** `paragraph`  
**Bundle :** `slide`  
**Label :** Slide  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Title|`paragraph.slide.field_title`|`paragraph.field_title`||
|Call to action|`paragraph.slide.field_text`|`paragraph.field_text`||
|Sub title|`paragraph.slide.field_subtitle`|`paragraph.field_subtitle`||
|Link|`paragraph.slide.field_node`|`paragraph.field_node`|Référence les contenu de type `page` uniquement.|
|Image|`paragraph.slide.field_media`|`paragraph.field_media`|Référence les media de type `image` uniquement.|

## sk_paragraph_text_image

### Dépendances

* `drupal:text`
* `drupal:options`
* `entity_browser:entity_browser`
* `entity_browser_entity_form:entity_browser_entity_form`
* `starter_kit:sk_commons`
* `starter_kit:sk_media_image`
* `paragraphs:paragraphs`
* `drupal:views`

#### Type de contenu

**Entity type :** `paragraph`  
**Bundle :** `image_text`  
**Label :** Image + Text  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Text|`paragraph.image_text.field_text`|`paragraph.field_text`||
|Media|`paragraph.image_text.field_media`|`paragraph.field_media`|Référence les media de type `image` uniquement.|

## sk_paragraph_title_text

### Dépendances

* `drupal:text`
* `starter_kit:sk_commons`
* `paragraphs:paragraphs`

#### Type de contenu

**Entity type :** `paragraph`  
**Bundle :** `title_text`  
**Label :** Title + Text  
**Champs :**  

|Label|Field|Storage|Détails|
|---|---|---|---|
|Title|`paragraph.title_text.field_title`|`paragraph.field_title`||
|Text|`paragraph.title_text.field_text`|`paragraph.field_text`||