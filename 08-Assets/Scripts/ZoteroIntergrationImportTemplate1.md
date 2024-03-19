---
aliases:
  - "{{citekey}}"
tags:
  - unread
rating: ⭐
share: false
ptype: article
annotation-target: "{% set regPdfLink = r/^\[.*\]\((.*)\)$/g %}{% set pdfLinkMatched = regPdfLink.exec(pdfLink) %}{{pdfLinkMatched[1]}}"
---
# {{title}}

## Title
{{title}}

## Authors
{% persist "authors" %}{% if isFirstImport %}
{% for a in creators %}
- {{a.lastName}} {{a.firstName}} ({{a.firstName}}@example.com, Affiliation)
{% endfor %}{% endif %}
{% endpersist %}

## Publication Year
{{date | format("yyyy")}}

{% persist "publications" %}{% if isFirstImport %}
## Conference or Journal
{% set regJournal = r/^.*(Journal|Transaction|Communication).*$/ig %}{% if regJournal.test(publicationTitle) %}Journal
{% else %}Conference
{% endif %}
## Conference or Journal Name
{{conferenceName}}{% set regConfName = r/^Conference Name: (.*?)\s+ISBN.*/g %}{% set extraMatched = regConfName.exec(extra) %}{% if extraMatched | length != 0 %}{% set regConfAbbr = r/^(\w+)\s+'?(\d+)\s*:\s*(\d+)?\s*(.*)$/g %}{% set confAbbrMatch = regConfAbbr.exec(extraMatched[1]) %}{% if confAbbrMatch | length != 0 %}{{confAbbrMatch[4]}}{% else %}{{ extraMatched[1]}}{% endif %}{% else %}{{publication}}{{proceedingsTitle}}{{series}}{% endif %}

## Abbreviation
{% if confAbbrMatch | length != 0 %}{{confAbbrMatch[1]}}{% endif %}
{%endif %}{% endpersist %}
## DOI
{{DOI}}

{% persist "misc" %}
## References


## Notes

{% endpersist %}

## Abstract
{{abstractNote}}

## TL;DR
{{markdownNotes}}

## Annotations
{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

{% for annotation in newAnnotations %}
### {{annotation.annotatedText}}
{{annotation.comment}}
{% if annotation.imageBaseName %}![[{{annotation.imageBaseName}}]]{% endif %}
{% endfor %}
{% endif %}
{% endpersist %}