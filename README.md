<!-- PROJECT LOGO -->
<br />
<p align="center">
  <h3 align="center">Dropdown menu multi level</h3>
  <p align="center">
    <a href="https://gitlab.com/Noradi/dropdown-menu-multi-level"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">View Demo</a>
    ·
    <a href="https://gitlab.com/Noradi/dropdown-menu-multi-level/-/issues">Report Bug</a>
    ·
    <a href="https://gitlab.com/Noradi/dropdown-menu-multi-level/-/issues">Request Feature</a>
  </p>
</p>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents </summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## About The Project

There are many great dropdown menu available on NPM, however, I didn't find one that really suit my needs so I created this enhanced one.

Here's why:

- You shouldn't spend your time to position the menu when is in the end of the screen. You can also use you own style.
- You can implement several menu levels. (each menu can have a sub menu up to infinity)
- You have the possibility to use it as a context menu ( in this case, the two characteristics mentioned above are also included)

[![Product Name Screen Shot][product-screenshot]]()
![](https://typito.com/share/qRJJdq8E)

<video width="550" height="240" controls>
  <source src="https://g-gs.typito.com/export/f2a87b2c-ad9c-4fa1-bfa1-82093a3c3b48/previews/dash/manifest.mpd" type="application/dash+xml">
</video>


<video width="550" height="240" controls>
  <source src="dropdown-video" type="video/mp4">
</video>

### Built With

- [VueJs](https://vuejs.org)
- [Bootstrap](https://getbootstrap.com)
- [Material Design Icons](https://material.io/resources)
- [bp-vuejs-dropdown](https://brandquad.github.io/bp-vuejs-demo/#/dropdown)

<!-- GETTING STARTED -->

## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Installation

1. NPM
   ```sh
   npm install dropdown-menu-multilevel
   ```
2. YARN
   ```sh
   yarn add dropdown-menu-multilevel
   ```

<!-- USAGE EXAMPLES -->

## Usage

- As dropdown

```html
<template>
  <DpMultilevel
    :list="listItems"
    className="my-class"
    label="Dropdown"
    @clickOnItem="clickOnItem($event)"
  >
  </DpMultilevel>
</template>

<script>
  import DpMultilevel from 'dropdown-menu-multilivel'
  export default {
    components: {DpMultilevel},
    data() {
      return: {
        listItems: [{
          label: "action 1",
          icon: "palette",
          align: "",
          children: [{
              label: "sub-action",
              className:"sub-action"
              children: [{
                icon: "edit"
                label: "sub-sub-action 1 ",
                children: []
                },
              {
                label: "sub-sub-action 2",
                children: []
              }]
            }],
          },
          {
           label: "action 2",
           icon: "delete",
           className: 'single',
           children: []
          },
        ]
      }
    },
    methods: {
       clickOnItem(el) {
      console.log("item app", el);
    },
    }
  }
</script>

<style lang="css">
  .my-class-bp__btn {
    background: lightgray;
    padding: 8px;
    border-radius: 8px;
    cursor: pointer;
  }
  .single {
    padding-left: 2em;
  }
  .my-class-bp__body {
    width: 12rem;
    padding: 0.6rem;
    font-size: 1rem;
    min-height: 5rem;
  }
  .sub-action-bp__btn {
    padding: 8px;
    cursor: pointer;
  }
  .sub-action-bp__body {
    width: 13rem;
    padding: 0.7rem;
    font-size: 1.3rem;
    min-height: 7rem;
  }
</style>
```

<!-- CONTRIBUTING -->

## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- LICENSE -->

## License

Distributed under the MIT License. See `LICENSE` for more information.

<!-- CONTACT -->

## Contact

Your Name - [@your_twitter](https://twitter.com/your_username) - email@example.com

Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)

<!-- ACKNOWLEDGEMENTS -->

## Acknowledgements

- [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
- [Img Shields](https://shields.io)
- [Choose an Open Source License](https://choosealicense.com)
- [GitHub Pages](https://pages.github.com)
- [Animate.css](https://daneden.github.io/animate.css)
- [Loaders.css](https://connoratherton.com/loaders)
- [Slick Carousel](https://kenwheeler.github.io/slick)
- [Smooth Scroll](https://github.com/cferdinandi/smooth-scroll)
- [Sticky Kit](http://leafo.net/sticky-kit)
- [JVectorMap](http://jvectormap.com)
- [Font Awesome](https://fontawesome.com)

https://gitlab.inf.rtbf.be/devtec/commons/js/vue/dropdown-menu.git

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[product-screenshot]: images/screenshot.png
[bp-vuejs-dropdown]: https://github.com/borisbutenko/bp-vuejs-dropdown
[bootstrap]: https://getbootstrap.com/
[material-design-icons]: https://material.io/resources/icons/
