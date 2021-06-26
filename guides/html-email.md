
Designing html email
==============================

Html email is just regular html code (instead of plain text), but you need to keep some stuff in mind:

- no separate css file, define all the styles in `<style>` tag within the head. You may copy/paste styles from the previous tasks (just don't forget to convert them to css!)
  

- since css styles are just limited to this html code and the html code is also not very big, so no need to bother with BEM or three-layer naming convention (or other css design systems). If you copy/paste from previous tasks you can keep the names, but feel free, it's just a simple html page.
  

- some html tags/structures are not supported by the email vendors. This is very tricky, I cannot find/give a list of these items but you need to try your html email in some vendors (yahoo, gmail, etc) and see if they look nice. The rule of thumb is: use SIMPLE SIMPLE HTML (like the one in 1990s). **Avoid using advanced css or html tweaks - stick to simple traditional html/css**. Note that email vendors are far behind the browsers for rendering html code.
  

- images should come from some URL (they cannot be saved/attached to the email). For our design, we just avoid images, and build the email only by text.
  

- probably you have lot of limitation to set custom fonts (for example gmail does not allow external fonts) - so in your design just go by the default font.

Try your html email by emailing to yourself (or maybe there are better ways). For example see [this guide](https://htmlemail.io/blog/how-to-send-html-emails-in-gmail) to learn how to use gmail with html emails.

For a sample of html emails you may just google *html email template*. There are tons! like [this one](https://unlayer.com/templates).

