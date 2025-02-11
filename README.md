# Pebble-Safe, a colourblind-safe RStudio theme

  - [Preview](#preview)
  - [Installation](#installation)
  - [Colourblind-friendly palettes](#colourblind-friendly-palettes)
  - [Testing the colours](#testing-the-colours)
      - [Try to programmatically remove similar colours under red-green
        blindness](#try-to-programmatically-remove-similar-colours-under-red-green-blindness)
      - [Manually select colours under red-green
        blindness](#manually-select-colours-under-red-green-blindness)
      - [Selected theme colours](#selected-theme-colours)
  - [Designing the themes](#designing-the-themes)
      - [1. Use a GUI editor to make a
        `.tmTheme`](#use-a-gui-editor-to-make-a-.tmtheme)
      - [2. Convert it to a `.rsTheme` in
        RStudio](#convert-it-to-a-.rstheme-in-rstudio)
      - [3. Edit the theme CSS by hand](#edit-the-theme-css-by-hand)

## Preview

Pebble-Safe is a colourblind-friendly theme for RStudio. It comes
in Light and Dark variants, but I recommend the Dark variant because it
makes better use of the few safe colours that are available. The font
used in the screenshots below is Hack in 10 pt
<https://sourcefoundry.org/hack>.

![](_img/dark.png)

![](_img/light.png)

## Installation

You need to be running RStudio v 1.2.x or greater.

First, install the `rstudioapi` package.

``` r
install.packages("rstudioapi")
```

Then, run the commands below to install the theme.

``` r
# Dark variant
rstudioapi::addTheme("https://raw.githubusercontent.com/robinweide/Pebble-safe/master/Pebble-Safe_Dark.rstheme",
                     apply = TRUE, force = TRUE)

# Light variant:
rstudioapi::addTheme("https://raw.githubusercontent.com/robinweide/Pebble-safe/master/Pebble-Safe_Light.rstheme",
                     apply = TRUE, force = TRUE)
```

You can change the active theme at any time by going to (Tools → Global
Options → Appearance) and applying a new theme.

The themes can be uninstalled by selecting them in the list and clicking
(Remove).

---

# Nerd info about how I chose a colourblind-friendly palette

Choosing colours for syntax highlighting has a few extra difficulties
compared to designing a poster or chart:

1.  Text at normal sizes has thinner outlines that make subtle colours
    harder to differentiate.
2.  Syntax highlighting is supposed to convey more information to the
    reader, and it’s hard to convey lots of information with a smaller
    palette of styles and still have each type of information clearly
    distinct from the others.
3.  The background of the text can affect the clarity of the colours.
    Yellow is one of the few safe colours out there, and yellow text on
    a white background is basically unreadable.

I found a few reliable sources for colourblind-friendly colour palettes.

This table of 8 colours is from Wong (2011).

> Wong, Bang. “Points of View: Color Blindness.” Nature Methods 8 (May
> 27, 2011): 441.  
> <https://doi.org/10.1038/nmeth.1618>.

![](_img/pointsofview.png)

This table of qualitative colors is from the `RColorBrewer` package.

> Erich Neuwirth (2014). RColorBrewer: ColorBrewer Palettes. R package
> version 1.1-2.  
> <https://CRAN.R-project.org/package=RColorBrewer>

``` r
display.brewer.all(type = "qual", colorblindFriendly = TRUE)
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

And this table of colours is from Sasha Trubetskoy.

> Trubetskoy, Sasha. List of 20 Simple, Distinct Colors.
> <https://sashat.me/2017/01/11/list-of-20-simple-distinct-colors>,
> accessed 2019-02-27.

``` r
sasha_cols <- c("#FFE119", "#4363D8", "#F58231", "#FABEBE", "#E6BEFF", 
                "#800000", "#000075", "#A9A9A9", "#FFFFFF", "#000000")

show_colours(sasha_cols)
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## Testing the colours

Here are all of the colours in one swatch. I’ve also added `#FCFCFC`, my
preferred light background colour, just a whisker off pure white so it’s
a little easier on the
eyes.

``` r
raw_cols <- c("#666666", "#A6761D", "#E6AB02", "#66A61E", "#E7298A", "#7570B3", 
              "#D95F02", "#1B9E77", "#A6CEE3", "#1F78B4", "#B2DF8A", "#33A02C", 
              "#FB9A99", "#E31A1C", "#FDBF6F", "#FF7F00", "#CAB2D6", "#6A3D9A", 
              "#FFFF99", "#B15928", "#B3B3B3", "#E5C494", "#FFD92F", "#A6D854", 
              "#E78AC3", "#8DA0CB", "#FC8D62", "#66C2A5", "#CD7EAA", "#D5683A", 
              "#2A78B5", "#F1E54E", "#2AA179", "#62B6E6", "#E7A337", "#292929",
              "#FFE119", "#4363D8", "#F58231", "#FABEBE", "#E6BEFF", "#800000", 
              "#000075", "#FCFCFC", "#A9A9A9")

show_colours(raw_cols)
```

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### Try to programmatically remove similar colours under red-green blindness

This is what the colour tiles above look like when deuteranopia (the
most common kind of colourblindness) is simulated with Color Oracle
<https://colororacle.org/>.

``` r
deut_cols <- c("#666666", "#868614", "#BEBE00", "#969626", "#888886", "#7171B2", 
               "#909000", "#888879", "#C3C3E3", "#6767B5", "#D2D28B", "#8A8A34", 
               "#BDBD95", "#838300", "#D3D36C", "#B0B000", "#B9B9D5", "#4E4E99", 
               "#FEFE99", "#7B7B21", "#B2B2B2", "#CECE93", "#E4E42A", "#CACA57", 
               "#ABABC0", "#9A9ACB", "#B6B65D", "#ADADA6", "#9A9AA8", "#939331", 
               "#6868B5", "#E8E84E", "#8A8A7B", "#A2A2E6", "#B9B931", "#2A2A2A", 
               "#E9E914", "#5B5BD8", "#ADAD21", "#D1D1BC", "#CACAFE", "#4A4A00", 
               "#141475", "#FBFBFB", "#A8A8A8")

show_colours(deut_cols)
```

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

I can take these deuteranopia colours, convert them to RGB space, round
their RGB scores to a common scale (e.g. round them to the nearest 10),
and programmatically eliminate colours that are at the same RGB point.

``` r
round_rgb <- function(hexcol, to = 1, dir = NULL) {
    col2rgb(hexcol) %>% 
        round_to_nearest(to = to, dir = dir) %>% 
        paste(collapse = " ")
}

cols <- 
    tibble(hex_col = raw_cols,
           hex_deut = deut_cols) %>% 
    rowwise() %>% 
    mutate_at(vars(starts_with("hex_")), 
              list(round = ~ round_rgb(., 10))) %>% 
    distinct(hex_deut_round, .keep_all = TRUE) %>% 
    distinct(hex_col_round, .keep_all = TRUE)

show_colours(cols$hex_col)
```

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

We eliminated these 3 colours
:

``` r
show_colours(c("#2A78B5", "#2AA179", "#A9A9A9"), main = "Excluded colours")
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

### Manually select colours under red-green blindness

``` r
show_colours(cols$hex_deut)
```

![](README_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

I hand-picked all of the colours that I could easily differentiate,
trying to maximise the differences between them. I made sure they were
easy to differentiate them by dragging them beside each other in my
colour picker (JCPicker <http://annystudio.com/software/colorpicker>)
and squinting at their differences.

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

### Selected theme colours

So I guess these are my theme colours\! These colours are okay under
simulated deuteranopia, protanopia, and
    tritanopia.

    ## Normal vision colours:

    ## [1] "#292929" "#4363D8" "#E7298A" "#E6BEFF" "#FFE119" "#FCFCFC" "#FFFF99"

    ## 
    ## Deuteranopic version:

    ## [1] "#E9E914" "#FEFE99" "#CACAFE" "#5B5BD8" "#2A2A2A" "#888886" "#FBFBFB"

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->![](README_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

## Designing the themes

(These are notes for the author and for any other theme authors who want
to know the process. If you are a user, you only need to follow the
[Installation instructions](#installation) above.)

It is often easier to visualise design changes by editing the stylesheet
live in the inspector. Right-click on the RStudio editor pane and choose
*Inspect Element*. In the left nav pane of the inspector, choose
*theme/custom/local → your .rsTheme file*. You can edit the settings
here but you cannot save the changes in the inspector.

### 1\. Use a GUI editor to make a `.tmTheme`

I use <https://tmtheme-editor.herokuapp.com>. The RStudio theme backend
has only some limited scopes, and the colourblind-friendly palette is so
limited that I can’t use all of them anyway.

These are the resources for designing a
    theme:

  - <https://support.rstudio.com/hc/en-us/articles/115011846747-Using-RStudio-Themes>
  - <https://rstudio.github.io/rstudio-extensions/rstudio-theme-creation.html>

#### Scope names that are actually used in RStudio

I compiled this list by editing the **3024 Day** theme in [TmTheme
Editor](https://tmtheme-editor.herokuapp.com), changing all of the
colours to a unique colour in a series (\#DDDD01, \#DDDD02, …). I
downloaded the `.tmTheme`, converted it to `.rsTheme` with
`rstudioapi::convertTheme()`, and then retrieved all of the colour codes
that were still present in the converted document.

Once that was done I extracted all of the scope names for the preserved
colours, put them into a new theme, and assigned each name a unique
colour from `desiderata::palette_distinct(spaced = FALSE)`. I downloaded
the theme, converted it, and then viewed one of my Rmarkdown files in
high zoom while using a colour picker tool to find out which of the
unique colours were actually used by
RStudio.

| tmTheme Editor tab | Scope             | GUI effect                                     | Code syntax highlighting                                                                    | Rmarkdown highlighting                                                                             |
| :----------------- | :---------------- | :--------------------------------------------- | :------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------- |
| General            | background        | Pane background colour.                        |                                                                                             |                                                                                                    |
| General            | caret             | Text cursor colour.                            |                                                                                             |                                                                                                    |
| General            | foreground        | All plain/unstyled text.                       | Argument names.                                                                             |                                                                                                    |
| General            | invisibles        | Invisible objects such as non-breaking spaces. |                                                                                             |                                                                                                    |
| General            | lineHighlight     | Current-line marker.                           |                                                                                             |                                                                                                    |
| General            | selection         | Selection box (when drag-selecting text).      |                                                                                             |                                                                                                    |
| Scopes             | comment           |                                                | Comments in code. Text in Roxygen blocks.                                                   |                                                                                                    |
| Scopes             | constant.language | Messages, errors, and warnings.                | TRUE, FALSE, NA, NULL, NaN, Inf, etc.                                                       | Italicised text (\*text\* or \_text\_).                                                            |
| Scopes             | constant.numeric  |                                                | Numbers.                                                                                    | Bold text (\*\*text\*\*).                                                                          |
| Scopes             | keyword           |                                                | Function names. Roxygen fields (e.g. @param).                                               |                                                                                                    |
| Scopes             | keyword.operator  |                                                | Brackets, assignment arrows, operators.                                                     |                                                                                                    |
| Scopes             | markup.heading    |                                                |                                                                                             | Headings, only the \# characters. The rest of the heading text must be edited in the .rsTheme CSS. |
| Scopes             | meta.tag          |                                                |                                                                                             | Metadata tags in YAML header (title, output, …)                                                    |
| Scopes             | string            |                                                | Character strings.                                                                          | Block quotes (\> text). Strings in YAML header.                                                    |
| Scopes             | support.class     |                                                | Package namespace names (dplyr::)                                                           |                                                                                                    |
| Scopes             | support.function  |                                                | Function names if “Highlight R function calls” is enabled. Argument names in Roxygen block. | Preformatted text (\`text\`). Code block headers. Special characters in YAML header.               |

### 2\. Convert it to a `.rsTheme` in RStudio

``` r
rstudioapi::convertTheme("src/Pebble-safe Light.tmTheme")
rstudioapi::convertTheme("src/Pebble-safe Dark.tmTheme")
```

### 3\. Edit the theme CSS by hand

Not much functionality is exposed via the editor theme scopes. These
features need to be added by hand.

These edits are made inside the converted `.rstheme` files. For me,
these files are located in `Documents\.R\rstudio\themes`. I edit them in
place and copy them to this repo to distribute them.

#### Fix heading style

Only the text colour of the markdown headings can be safely changed.
Changing the background colour masks the selection box, and changing the
text size makes the cursor no longer line-up with the text on that
header line.

``` css
.ace_markup.ace_heading {
  text-decoration: underline;
  color: #REPLACEME;
}

.ace_heading {
  text-decoration: underline;
  color: #REPLACEME;

}
```

#### Matching bracket indicator

Make it magenta to match the cursor.

``` css
.ace_bracket {
  margin: 0 !important;
  border: 0 !important;
  background-color: #CURSORCOLOUR;
}
```

#### Markdown block background

I made the background of the markdown blocks more similar to the normal
background, and I added borders on the left and right sides of the block
to delineate it.

I ideally would have liked to have a line at the top and bottom of the
block, but the style is applied on a per-line basis and the block itself
is not a single box element.

    .ace_marker-layer .ace_foreign_line {
      position: absolute;
      z-index: -1;
      background-color: #fdfdfd; for light
      background-color: #2d2d2d; for dark
      border-right: 2px solid #cccccc;
      margin-right: 5px;
    }

#### Column margin line

The default is too bright in the Dark variant.

    .ace_print-margin {
      width: 1px;
      background: #424242;
    }
