title: "<%= it.title %>"
citekey: "<%= it.citekey %>"
tags: [ unread, <%= it.tags %> ]
rating: ⭐
share: false
ptype: article
annotation-target: <%-
  const regPdfLink = /^\[.*\]\(<(.*)>\)$/g;
  var pdfLinkMatched = regPdfLink.exec(it.fileLink);
-%><%= pdfLinkMatched?.at(1) %>