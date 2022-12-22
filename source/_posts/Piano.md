---
title: How to Build a Piano With Only HTML, CSS, Javascript ?
date: 2022-12-15 18:54:29
tags: 
  - typography
  - hexo
---
![$cover](images/code.webp)

In this post, we’ll learn how to build [this piano](https://javascript-piano-lotfijb.netlify.app) from scratch using simple HTML, CSS, and JavaScript.

Firstly, we’ll structure our web app using HTML. We’ll use the main tag to make a wrapper structure for the piano.

We will create classes to show white and black keys, and classes will be further used in CSS for designing.

I added a class empty to some black keys because we don't need the same amount of white keys and I tried to do it the simple way.

In script.js, I grabbed all keys with className to make an array of all keys and created an array of sounds.

I, then, iterated through every single element of array keys and added a click event to play a random sound.

I, also, created a play() function to play a random sound on every keyboard button press on body element (I don't recommend that but I wanted to make it simple).

I created a sounds folder where I stored 21 different sounds of piano keys.

[](#indexhtml)index.html
------------------------

[![HTML](https://res.cloudinary.com/practicaldev/image/fetch/s--AqHrw2rS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x8qju1wrknh426chh6en.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--AqHrw2rS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x8qju1wrknh426chh6en.png)

[](#stylecss-click-to-zoom-)style.css ( click to zoom )
-------------------------------------------------------

[![CSS](https://res.cloudinary.com/practicaldev/image/fetch/s--0f-_9Uhq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9196a4jg6rna5c89r9e8.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--0f-_9Uhq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9196a4jg6rna5c89r9e8.png)

[](#scriptjs)script.js
----------------------

[![JS](https://res.cloudinary.com/practicaldev/image/fetch/s--fY-2ba_Z--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ptfpfqkuaiamqrsjukb6.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--fY-2ba_Z--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ptfpfqkuaiamqrsjukb6.png)

NOTE :  
There is room for improvement and this is only for beginners who just started as it covers simple concepts.