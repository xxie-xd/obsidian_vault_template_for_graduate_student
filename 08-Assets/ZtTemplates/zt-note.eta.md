# <%= it.title %>

[Zotero](<%= it.backlink %>) <%= it.fileLink %>

## Title
<%= it.title %>

## Authors
<%- it.authors.forEach(author => { %>
- <%= author.fullname -%> (<%= author.firstName%>@example.com, Affiliation)
<%- }) %>

## Publication Year
<% let dateStr = it.date; var date = new Date(dateStr); var year = date.getFullYear();  -%>
<%= year %>

## Conference or Journal
<% 
  const regJournal = /^.*(Journal|Transaction).*$/ig ;
  if (regJournal.test(it.publicationTitle)) {
    %>Journal<%
  } else { 
    %>Conference<%
  }
%>
## Conference or Journal Name
<%
  const regConfName = /^Conference Name: (.*?)\s+ISBN.*/g ;
  var regConfAbbr = new RegExp("^(.*) '?[0-9]+:.([0-9]+)?.(.*?)\n?$","g") ;
  var confName = it.conferenceName;
  if (!it.conferenceName && it.extra) {
    var extraMatched = regConfName.exec(it.extra);
    if (extraMatched ) {
      confName = extraMatched[1];
    }
  }
  var confAbbrMatched = regConfAbbr.exec(confName);  
  if (confAbbrMatched) {    
    %><%= confAbbrMatched[3] %><% 
  } else if (confName) { 
    %><%= confName %><%
  } else {
    %><%= it.publication ?? it.proceedingTitle ?? it.series %><%
  }
%>

## Abbreviation
<%= confAbbrMatched?.at(1) ?? "Abbreviation" %>

## DOI
<%= it.DOI %>

## References

## Notes

## Abstract
<%= it.abstractNote %>

## TL;DR
<%= it.notes?.at(0) %>

## Annotations
<%~ include("annots", it.annotations) %>