<!--
================================================================================
ZOTERO → OBSIDIAN ADVANCED TEMPLATE (COMMENTED VERSION)
================================================================================

This template is designed to work with:
- Obsidian
- Zotero
- Zotero Integration plugin (Nunjucks-based templating)

IMPORTANT:
- Sections marked with [USER] are meant to be filled manually
- Sections marked with [AUTO] are automatically generated
- Comments like this one do NOT affect functionality

Inspired by:
Alexandra Phelan – “An Updated Academic Workflow: Zotero & Obsidian”

================================================================================
-->

---
tags:
last date: "" # [USER] Last time you worked on this note
creation date: "" # [USER] Creation date of the note
category:
  - "[[Zotero]]"
  - "[[Saberes/Información/Información]]"
  - "[[Saberes]]"
citekey:
  - "{{citekey}}" # [AUTO] Zotero citation key
related:
---

<!-- ========================= -->
<!-- 📚 ZOTERO METADATA [AUTO] -->
<!-- ========================= -->

> [!Zotero] 
> **Zotero_Tags** :: {% if tags %} {%- for t in tags -%} {% if t.type is not defined %} #{{ t.tag }} {% endif %} {%- endfor -%} {% endif %}

<!-- ========================= -->
<!-- 🧠 ANKI INTEGRATION -->
<!-- ========================= -->

>[!Anki]-
> **Anki_Deck** :: Fields of Knowledge::  
> **Anki_Tags** :: Source::{{citekey}} Level:: Research:: Fields_of_Knowledge::  
> **Anki_Fields** - MarcoMol | Cloze Study:  
Explanation:  <!-- [USER] Add explanation for cloze -->
Image:  <!-- [USER] Optional image -->
LinkText: {{bibliography}}  
LinkURL: {{url}}

<!-- ========================= -->
<!-- 📖 CITATION [AUTO] -->
<!-- ========================= -->

> [!Citation] 
> {{bibliography}}

<!-- ========================= -->
<!-- 🧩 SYNTHESIS -->
<!-- ========================= -->

>[!Synthesis] 
> **Contribution** :: <!-- [USER] What does this work contribute? -->
>
> **Related** :: {% for relation in relations | selectattr("citekey") %} [[@{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}

<!-- ========================= -->
<!-- 📊 METADATA [AUTO] -->
<!-- ========================= -->

>[!Metadata] 
{% for type, creators in creators | groupby("creatorType") -%}
{%- for creator in creators -%}
> **{{"First_" if loop.first}}{{type | capitalize}}** ::
{%- if creator.name %} {{creator.name}}  
{%- else %} {{creator.lastName}}, {{creator.firstName}}  
{%- endif %}  
{% endfor %}~ 
{%- endfor %}    
> **Title** :: {{title}}  
> **Year** :: {{date | format("YYYY")}}   
> **Citekey** :: {{citekey}} {%- if itemType %}  
> **Item_Type** :: {{itemType}}{%- endif %}{%- if itemType == "journalArticle" %}  
> **Journal** :: *{{publicationTitle}}* {%- endif %}{%- if volume %}  
> **Volume** :: {{volume}} {%- endif %}{%- if issue %}  
> **Issue** :: {{issue}} {%- endif %}{%- if itemType == "bookSection" %}  
> **Book** :: {{bookTitle}} {%- endif %}{%- if publisher %}  
> **Publisher** :: {{publisher}} {%- endif %}{%- if place %}  
> **Location** :: {{place}} {%- endif %}{%- if pages %}   
> **Pages** :: {{pages}} {%- endif %}{%- if DOI %}  
> **DOI** :: {{DOI}} {%- endif %}{%- if ISBN %}  
> **ISBN** :: {{ISBN}} {%- endif %}

<!-- ========================= -->
<!-- 🔗 FILE LINKS [AUTO] -->
<!-- ========================= -->

> [!Link] 
> {%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}
>  [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}})
>  {%- endfor %}

<!-- ========================= -->
<!-- 🧾 ABSTRACT -->
<!-- ========================= -->

> [!Abstract]
> {%- if abstractNote %}
> {{abstractNote}}
> {%- else %}
> Add abstract...
> {%- endif %}

***
***

# ✍️ Comments
<!-- [USER] Personal reflections, interpretations, or key insights -->
- Any important information or personal elaboration on the text.

***
***

## Section
<!-- [USER] Optional structured breakdown -->

1. Comments.

***
***

# 🧠 Schemes
<!-- [USER] Visual thinking (diagrams, structures, etc.) -->

- This section is dedicated to structured visual representations of key concepts.

***
***

## Mindmap
```mermaid
```

***
***

# 📚 Summary
<!-- [USER] Rewrite the content in your own words -->

- Here is a summary in our own words of the most relevant information.

---

## «{{title | title}}» Main Takeaways

1. Write a brief summary of each section or paragraph.

---

## Connections

2. Identify how the current paper connects to your research or other papers.

---

## Curiosities & Comments

3. Any personal comments, ideas or interesting facts.

---

## Definitions

4. Note any general definitions or concepts that arise as you read.

---

## Questions

1. Note the questions that the paper inspired.

{%- if markdownNotes %}

***
## Other Notes
<!-- [AUTO / USER] Imported or extra notes -->

{{markdownNotes}}
{%- endif %}

***
***

<!--
================================================================================
🧾 THIS ANNOTATION SYSTEM EXPLAINED (ZOTERO → OBSIDIAN)
================================================================================

This section explains how annotations from Zotero are processed and rendered
in Obsidian based on this highlighting workflow:

- Annotation TYPE (highlight, underline, note, image)
- Annotation COLOR (Yellow, Blue, Red, Green, etc.)

The system allows semantic encoding of meaning through color.

--------------------------------------------------------------------------------
🎨 COLOR → MEANING MAPPING
--------------------------------------------------------------------------------

Gray:
- Structural highlights
- Rendered as headings
  - Highlight → ### Heading
  - Underline → #### Subheading

Yellow:
- Core content / main ideas
- Highlight → bold bullet point title
- Underline → numbered list content
- Notes → own numbered list content (same as underline, but with admonition possibility)
- Images → embedded image

Purple:
- Highlight → numbered list content (for older versions of Zotero that do not include an underline option)

Magenta:
- Mathematical / formal content (To render them, you must enable the corresponding LaTeX add-on)
- Highlight → LaTeX block ($$ ... $$)
- Underline → Inline LaTeX ($...$)

Other colors (semantic callouts/admonitions blocks, must be configured in Obsidian):

Orange → [!Connection]
- Used for linking ideas

Green → [!Curiosity]
- Interesting insights or side ideas

Blue → [!Definition]
- Definitions or key concepts

Red → [!Question]
- Doubts, open questions

--------------------------------------------------------------------------------
🧩 ANNOTATION TYPES
--------------------------------------------------------------------------------

highlight:
- Main highlighted text in Zotero

underline:
- Underlined text (used for structure or ordered ideas)

note:
- Own comments attached to annotations

image:
- Extracted images from PDF

--------------------------------------------------------------------------------
⚙️ PROCESSING LOGIC
--------------------------------------------------------------------------------

1. Only NEW annotations are imported:
   → filtered by `lastImportDate`

2. Each annotation is processed based on:
   → type (highlight, underline, etc.)
   → colorCategory (Yellow, Blue, etc.)

3. Formatting rules are applied conditionally:
   → using Nunjucks templating

4. Non-standard colors trigger callouts:
   → via `calloutHeader()` macro

--------------------------------------------------------------------------------
🧠 DESIGN PRINCIPLE
--------------------------------------------------------------------------------

The system turns highlighting into structured knowledge:

- Colors = semantic meaning
- Types = structural role
- Output = ready-to-study notes (it might need a basic cleanup; AI can help)

This allows:
✔ Fast reading
✔ Automatic structuring
✔ Direct integration into summaries & Anki

--------------------------------------------------------------------------------
🔧 CUSTOMIZATION
--------------------------------------------------------------------------------

To adapt the system:

- Change color mappings inside `calloutHeader()`
- Modify formatting rules in the `{% if %}` blocks
- Add new categories (e.g., Argument, Example, Critique)

--------------------------------------------------------------------------------
-->

# 🧾 Annotations
<!-- [AUTO] Imported highlights and notes from Zotero -->

- This section contains highlights, underlines, and notes.

***
***

{% macro calloutHeader(type, colorCategory) -%}  
{%- if colorCategory == "Orange" -%}
[!Connection]
{%- endif -%}

{%- if colorCategory == "Green" -%}
[!Curiosity]
{%- endif -%}  

{%- if colorCategory == "Blue" -%}
[!Definition]
{%- endif -%}

{%- if colorCategory == "Red" -%}
[!Question]
{%- endif -%}
{%- endmacro -%}

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}

{% if newAnnotations.length > 0 %}

###### Imported: {{importDate | format("YYYY-MM-DD h:mm a")}}


{% for a in newAnnotations %}

{%- if a.type == "highlight" and a.colorCategory == "Gray" %}### {{a.annotatedText | title}}
{% endif -%}

{%- if a.type == "underline" and a.colorCategory == "Gray" %}#### {{a.annotatedText | title}}
{% endif -%}

{%- if a.type == "highlight" and a.colorCategory == "Yellow" %}- **{{a.annotatedText | replace((a.annotatedText | first), (a.annotatedText | first | upper), 1)}}**...
{%- endif -%}

{%- if a.type == "underline" and a.colorCategory == "Yellow" %}    1. {{a.annotatedText | replace((a.annotatedText | first), (a.annotatedText | first | upper), 1)}}.
{%- endif -%}

{%- if a.comment %}    1. {{a.comment | replace((a.comment | first), (a.comment | first | upper), 1)}}
{%- endif -%}

{%- if a.type == "note" and a.colorCategory == "Yellow" %}    1. {{a.comment | replace((a.comment | first), (a.comment | first | upper), 1)}}
{%- endif -%}

{%- if a.type == "highlight" and a.colorCategory == "Purple" %}    1. {{a.annotatedText | replace((a.annotatedText | first), (a.annotatedText | first | upper), 1)}}.
{%- endif -%}

{%- if a.type == "underline" and a.colorCategory == "Magenta" %}${{a.annotatedText}}$
{%- endif -%}

{%- if a.type == "highlight" and a.colorCategory == "Magenta" %}$$\begin{gather*}
{{a.annotatedText}}
\end{gather*}$$
{%- endif -%}

{%- if a.type == "image" and a.colorCategory == "Yellow" %}
{{"    !" if a.imageBaseName}}[[{{a.imageBaseName}}]]
{%- endif -%}

{%- if a.colorCategory != "Yellow" and a.colorCategory != "Gray" and a.colorCategory != "Purple" and a.colorCategory != "Magenta" %}
> {{calloutHeader(a.type, a.colorCategory)}}
{%- if a.type == "highlight" %}
>- **{{a.annotatedText | replace((a.annotatedText | first), (a.annotatedText | first | upper), 1)}}**...

{% endif %}
{%- if a.type == "underline" %}
>    1. {{a.annotatedText | replace((a.annotatedText | first), (a.annotatedText | first | upper), 1)}}.

{% endif %}
{%- if a.type == "note" %}
> *{{a.comment}}*

{% endif %}
{%- if a.type == "image" %}
> {{"!" if a.imageBaseName}}[[{{a.imageBaseName}}]]

{% endif %}

{%- endif %}
{% endfor %}

{% endif %}
{% endpersist %}
