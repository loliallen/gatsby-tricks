# gatsby-tricks

## Gatsby with MUI@v5.*

### `gatsby-config.js`
```js
plugins: [
"gatsby-plugin-sass",
    "gatsby-plugin-split-css",
    "gatsby-plugin-react-helmet",
    "gatsby-transformer-json",
    {
      resolve: "gatsby-source-filesystem",
      options: {
        name: "data",
        path: "src/data",
      },
    },
    "gatsby-transformer-sharp",
    {
      resolve: "gatsby-plugin-sharp",
      options: {
        defaults: {
          formats: ["auto", "webp", "avif"],
          placeholder: "blurred",
          quality: 50,
        },
      },
    },
    "gatsby-plugin-image",
    {
      resolve: "gatsby-source-filesystem",
      options: {
        name: "images",
        path: "src/images",
      },
    },
    {
      resolve: "gatsby-plugin-manifest",
      options: {
        name: "Совкомбанк",
        short_name: "Совкомбанк",
        lang: "ru",
        start_url: ".",
        background_color: "#003791",
        theme_color: "#003791",
        display: "standalone",
        icon: "src/images/logo512.png",
      },
    },
    "gatsby-plugin-remove-console",
    {
      resolve: "gatsby-plugin-material-ui",
      options: {
        stylesProvider: {
          injectFirst: true,
        },
        disableAutoprefixing: true,
      },
    },
]
```
### `gatsby-ssr.js`
```js
export const onPreRenderHTML = ({ pathname, getHeadComponents, replaceHeadComponents }) => {
  if (process.env.NODE_ENV === "production") {
    const headComponents = getHeadComponents()

    headComponents.sort((x, y) => {
      if (
        y.props?.id === "gatsby-global-css" ||
        y.props?.["data-identity"] === "gatsby-global-css"
      ) {
        return -1
      } else if (["gatsby-image-style", "jss-server-side"].includes(x.key)) {
        return 1
      }

      return 0
    })

    // let replacedHeadComponents
    // if (pathname.indexOf("nonegtm") > -1) {
    //   replacedHeadComponents = headComponents.filter((x) => x.key !== "plugin-google-tagmanager")
    // }

    replaceHeadComponents(headComponents)
  }
}
```
