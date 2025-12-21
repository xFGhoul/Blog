# Blog

Source Code of Personal dev blog for https://blog.ghoul.cloud

## Commands

| Command          | Action                                                         |
| :--------------- | :------------------------------------------------------------- |
| `bun install`   | Installs dependencies                                          |
| `bun dev`       | Starts local dev server at `localhost:3000`                    |
| `bun build`     | Build your production site to `./dist/`                        |
| `bun postbuild` | Pagefind script to build the static search of your blog posts  |
| `bun preview`   | Preview your build locally, before deploying                   |
| `bun sync`      | Generate types based on your config in `src/content/config.ts` |

## Adding posts, notes, and tags

This theme uses [Content Collections](https://docs.astro.build/en/guides/content-collections/) to organise local Markdown and MDX files, as well as type-checking frontmatter with a schema -> `src/content.config.ts`.

Adding a post/note/tag is as simple as adding your .md(x) files to either `src/content/post`, `src/content/note`, and `src/content/tag` folders, the filename of which will be used as the slug/url.

The Tag collection allows you to override the content for generated tag pages. For example the template includes `src/content/tag/test.md` which overrides the content shown in `your-domain.com/tags/test`.

> **Note**
> For a tag page to work, the file name (`src/content/tag/*`) must also be in a post's [tags frontmatter.](#post-frontmatter)

The posts/notes/tags included with this template are there as an example of how to structure your frontmatter. Additionally, the [Astro docs](https://docs.astro.build/en/guides/markdown-content/) has a detailed section on markdown pages.

### Post Frontmatter

| Property (\* required) | Description                                                                                                                                                                                                                                                                                                  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| title \*               | Self explanatory. Used as the text link to the post, the h1 on the posts' page, and the pages title property. Has a max length of 60 chars, set in `src/content/config.ts`                                                                                                                                   |
| description \*         | Similar to above, used as the seo description property. Has a min length of 50 and a max length of 160 chars, set in the post schema.                                                                                                                                                                        |
| publishDate \*         | Again pretty simple. To change the date format/locale, currently **en-GB**, update the date option in `src/site.config.ts`. Note you can also pass additional options to the component `<FormattedDate>` if required.                                                                                        |
| updatedDate            | This is an optional date representing when a post has been updated, in the same format as the publishDate.                                                                                                                                                                                                   |
| tags                   | Tags are optional with any created post. Any new tag(s) will be shown in `your-domain.com/posts` & `your-domain.com/tags`, and generate the page(s) `your-domain.com/tags/[yourTag]`                                                                                                                         |
| coverImage             | This is an optional object that will add a cover image to the top of a post. Include both a `src`: "_path-to-image_" and `alt`: "_image alt_". You can view an example in `src/content/post/cover-image.md`.                                                                                                 |
| ogImage                | This is an optional property. An OG Image will be generated automatically for every post where this property **isn't** provided. If you would like to create your own for a specific post, include this property and a link to your image, the theme will then skip automatically generating one.            |
| draft                  | This is an optional property as it is set to false by default in the schema. By adding true, the post will be filtered out of the production build in a number of places, inc. getAllPosts() calls, og-images, rss feeds, and generated page[s]. You can view an example in `src/content/post/draft-post.md` |

### Note Frontmatter

| Property (\* required) | Description                                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------- |
| title \*               | Used as the link text to the note, the pages title property, and the h1 of said note page. Has a max length 60 chars. |
| description            | Optional. Used for the head meta description property.                                                                |
| publishDate \*         | ISO 8601 format with offsets allowed.                                                                                 |

### Tag Frontmatter

| Property (\* required) | Description                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------- |
| title                  | Optional. Used as the h1 on the tags' page, and the pages title property. Has a max length of 60 chars. |
| description            | Optional. Used for the head meta description and the first paragraph under the h1.                      |

### Frontmatter snippets

Astro Cactus includes a helpful VSCode snippet which creates a frontmatter 'stub' for posts and note's, found here -> `.vscode/post.code-snippets`. Start typing the word `frontmatter` on your newly created .md(x) file to trigger it. Visual Studio Code snippets appear in IntelliSense via (⌃Space) on mac, (Ctrl+Space) on windows.

## Pagefind search

This integration brings a static search feature for searching blog posts and notes. In its current form, pagefind only works once the site has been built. This theme adds a postbuild script that should be run after Astro has built the site. You can preview locally by running both build && postbuild.

Search results only includes pages from posts and notes. If you would like to include other/all your pages, remove/re-locate the attribute `data-pagefind-body` to the article tag found in `src/layouts/BlogPost.astro` and `src/components/note/Note.astro`.

It also allows you to filter posts by tags added in the frontmatter of blog posts. If you would rather remove this, remove the data attribute `data-pagefind-filter="tag"` from the link in `src/components/blog/Masthead.astro`.

If you would rather not include this integration, simply remove the component `src/components/Search.astro`, and uninstall both `@pagefind/default-ui` & `pagefind` from package.json. You will also need to remove the postbuild script from here as well.

You can reduce the initial css payload of your css, [as demonstrated here](https://github.com/chrismwilliams/astro-theme-cactus/pull/145#issue-1943779868), by lazy loading the web components styles.

## Analytics

You may want to track the number of visitors you receive to your blog/website in order to understand trends and popular posts/pages you've created. There are a number of providers out there one could use, including web hosts such as [vercel](https://vercel.com/analytics), [netlify](https://www.netlify.com/products/analytics/), and [cloudflare](https://www.cloudflare.com/web-analytics/).

This theme/template doesn't include a specific solution due to there being a number of use cases and/or options which some people may or may not use.

You may be asked to included a snippet inside the **HEAD** tag of your website when setting it up, which can be found in `src/layouts/Base.astro`. Alternatively, you can add the snippet in `src/components/BaseHead.astro`.
