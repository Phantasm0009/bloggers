---
title: Simple Search Box With Icon
date: 2022-12-17 11:06:07
tags:
    - Search
    - Html
---

It's like this:

[![Image description](https://res.cloudinary.com/practicaldev/image/fetch/s--W7qvCPMG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eyknz8xfm7e5d5qcag7t.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--W7qvCPMG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eyknz8xfm7e5d5qcag7t.png)

Here is the code:  

    
    <input class="search" type="search" placeholder="Search...">
    
    

Enter fullscreen mode Exit fullscreen mode

        input.search {
            width: 260px;
            border: 1px solid #555;
            display: block;
            padding: 9px 4px 9px 40px;
            background: transparent url("/assets/search.svg") no-repeat 13px;
        }
    

Enter fullscreen mode Exit fullscreen mode

`padding` top, left, bottom, right - Places `placeholder` named `Search` just beside icon inside input.

download `svg` from any icon store say [bootstrap](https://icons.getbootstrap.com/icons/search/) and place inside folder.

`background` property - color, image, repeat, position - Places search icon in proper place in search box.

Thanks.