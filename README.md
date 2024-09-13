# Infuse website 

The [Infuse website](https://infuse.quest) is built using the [Hugo](https://gohugo.io) Static Site Generator.
To edit the website locally, we have to install Hugo Extended version and NPM/NodeJS (latest version preferrable).

## Overview

Hugo is a static HTML and CSS website generator written in Go. It is optimized for speed, ease of use, and configurability. Hugo takes a directory with content and templates and renders them into a full HTML website.

Hugo relies on Markdown files with front matter for metadata, and you can run Hugo from any directory. This works well for shared hosts and other systems where you don’t have a privileged account.

Hugo renders a typical website of moderate size in a fraction of a second. A good rule of thumb is that each piece of content renders in around 1 millisecond.

## How to install

If you want to use Hugo as your site generator, simply install the Hugo binaries. Use the [installation instructions in the Hugo documentation](https://gohugo.io/getting-started/installing/).

You can install NodeJS/NPM via their [official website](https://nodejs.org/en/download/), or via your local package manager if you're using a *nix operating system.

## Local testing

After installing NodeJS, we have to run the following command inside the root path of the website.

`npm install`

This will install the required modules we use to tree-shake the CSS files and make them smaller, greatly reducing loading size and time.

Afterwards, we have to run Hugo with the following command.

`hugo server --environment production`

The `--environment` argument is necessary for us to be able to render the content in the same way that the public version would come out to be.

After this step, we have to run the TailwindCSS watcher and builder process in case we want to edit add/remove/edit classes in the HTML files/layouts. 

`npm run watch-tw`

To be able to use the CMS locally, we need to first install it and then make a few changes to the `static/admin/config.yml` file in order to enable local changes without having to go through the Git repository.

`npx decap-server`

The change we have to make is to comment out the `backend repo` and the `backend branch` lines, and set the `local_backend` line to true. With this done, we can use the CMS locally for an easier edit flow. The main downside of this change is that we can't use the editorial workflow anymore, so all our changes would be applied instantly (we will not have drafts anymore). We can now access the access locally in the following URL:

`http://localhost:1313/admin`

Additionally, in order to build the search index we have to run the following command that will hot-build the search index for us.

`npx -y pagefind --site public --serve`

With this we're ready to start editing the website. If we do not want to make changes to the CSS or use the CMS locally, we can just run the `hugo server` command and start editing the markdown files.

## Docs

For further changes and customization, refer to the [Hugo docs](https://gohugo.io/documentation/).

## How to create and edit content

### Creating New Pages:

To generate new pages, a new markdown file with the extension `.md` should be introduced within the `content` directory. It is imperative to place these items within the designated folders in order to generate them with the correct path/slug.

For the creation of new learning paths, one must create a new folder with the learning path number inside the `content/learning-path` folder, and then a `_index.<LanguageCode>.md` file, employing the structure found in existing learning paths as a reference.

For the creation of new modules, one must insert new markdown files into the `content/learning-path/<number>` folder, naming the files as `module-<number>.<LanguageCode>.md` and then employing the structure found in existing modules as a reference.

### Front Matter for Markdown Files:

Each markdown file is equipped with a front matter, which holds pivotal information regarding the respective page. Notably, the most crucial elements to consistently include within the front matter are: type, title, description, date, images and categories. If the page in question is destined to adopt a single-page layout, akin to the privacy or imprint pages, the inclusion of layout and type metadata may be omitted, as they are deemed unnecessary for rendering the page.

To edit the navigation bar and footer navigation items, we can set the `navbar` and `footer` variables as true in the markdown files for single pages such as About/Contact/Contribute.

To edit the Frequently Asked Questions, we can use the CMS or edit the files inside the `content/faq` folder, by adding/editing/removing question and answer pairs.

### Internationalization keys

Translation/i18n keys are located in the `i18n` folder, inside `.yml` files. These can be edited from the CMS or directly in the file as well. In order to create a new key, we can use the existing structure and then reference them in the HTML files as `{{ i18n "key.<id>" }}`.

### Modular Template Structure:

The templates within this project are systematically constructed to facilitate editing and the prospect of future additions. The primary layouts are established within the `layouts/_default` folder. These templates integrate items from the `layouts/partials` folder, whose nomenclature elucidates their intended purpose. Editing these components is not obligatory, unless there is a desire to create novel layouts or make alterations to existing ones.

### Styling Framework and Custom Styling:

The styling elements are stored within the `assets` folder, located in the `tailwind.css` file. Here we can insert custom CSS that is not available by default on TailwindCSS.

These guidelines should assist in the effective management and customization of content within the Hugo project.

### Testing and Quality Assurance:

Before deploying changes to your live website, it is prudent to thoroughly test your content and layouts on a staging or development server. Ensure that all pages render correctly and that links and functionality are intact.
