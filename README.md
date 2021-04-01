# spieswl.github.io

## Built with Jekyll

This website is intended to be hosted on GitHub Pages, which uses [Jekyll](http://jekyllrb.com/docs/home/) to regenerate the website after changes occur in this repository. As such, you can use Jekyll on a local clone of this repository to modify and preview changes before pushing them to a hosting provider.

### Local Previewing

First, you must have Ruby and Jekyll installed...

#### Debian-based Linux

From a command prompt...

```bash
sudo apt install ruby ruby-dev jekyll
```

#### Windows-based Operating Systems

I recommend following the first set of instructions on Jekyll's website for [installing Ruby and Jekyll](https://jekyllrb.com/docs/installation/windows/). Once you can query Jekyll versions from a vanilla command or PowerShell prompt, you should be good to continue

After installation is complete, locally clone this repository and navigate to the site's root level. Then build the static site using __Jekyll__ from a terminal or PowerShell session...

```bash
jekyll build --watch
```

In a different terminal, start a local server (this must also be run in site's root directory)...

```bash
jekyll serve
```

Finally, view the site in your browser at `localhost:4000`. Changes you make to template files will be reflected in changes to the static site _(after a short delay)_.

## License

Original pictures, code, documents, or other non-website-related content is covered by a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/).

The website infrastructure (everything other than original content covered above) is covered under [MIT License](https://opensource.org/licenses/MIT).
