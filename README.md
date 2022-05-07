# Bullshit problems with {gnome|emacs|popos|ubuntu} and the weird hacks to get around them:

Each of these throws me some weird, obscure and unintuitive error at the combined rate of ~1 per week. Often, the same issue reappears after updating either one of these. So, rather than keep having to go down the search rabbit hole on GH. I want to keep track of the recurring problems here so that I may find the "fixes" more quickly in the future. 

# [Pop!_OS](https://github.com/pop-os) 

# [Emacs]()
## Exporting to LaTex:
### SVG:

**situation:** during normal workflow when working with an `.org` file which I'm frequently iterating on and exporting to LaTex/HTML I included an svg image in the document and it wouldn't display it. 

**problem:** stupid line of code from the developers of ox.el (emacs exporting framework for org mode) where they apply a regex which strips out any file extension that is not on their list:

```elisp
(defconst org-export-default-inline-image-rule
  `(("file" .
     ,(format "\\.%s\\'"
	      (regexp-opt
	       '("png" "jpeg" "jpg" "gif" "tiff" "tif" "xbm" 
		 "xpm" "pbm" "pgm" "ppm") t))))
```
Thus, the resulting org->latex ends up with \includesvg{filename} instead of \include{filename.svg}. 

**"fix":**

```elisp
(defconst org-export-default-inline-image-rule
  `(("file" .
     ,(format "\\.%s\\'"
	      (regexp-opt
	       '("png" "jpeg" "jpg" "gif" "tiff" "tif" "xbm" "svg"
		 "xpm" "pbm" "pgm" "ppm") t))))
```
then save and reload configs.

**time-wasted-debugging:** ~30 minutes

