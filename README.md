# 945 Documentation

### Web -- https://ecosoft-odoo.github.io/945/_build/html

### PDF -- https://ecosoft-odoo.github.io/945/_build/latex/945manuals.pdf

### Presentation -- https://ecosoft-odoo.github.io/945/_build/slides/technical.html#1

---

Steps to setup this document using https://github.com/nyergler/hieroglyph

* easy_install hieroglyph (an sphinx extension)
* Som dependencies, otherwise, can't create PDF
```
> sudo apt-get install texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
```
* Create "docs" folder and inside it run command to generate default assets
```
> hieroglyph-quickstart
```
* .gitignore
  * remove `# docs/_build/`
  * add `!.nojekyll`, this file is requried to ensure that github page can use files in sub-folder.
* Add file `.nojekyll` in "docs" folder

Create / Update documentation, i.e., index.rst and all its component files.

When ready, run following commands in "docs" folder, to generate documents in others format.

```
> make html
> make slides
> make latexpdf
```

Sample: https://github.com/kmee/oca-days-2020-odoo-developer/blob/docs/docs/index.rst