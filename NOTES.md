UNDER THE HOOD OF MODERN CSS FRAMEWORKS: NOTES
----------------------------------------------

1. Resets
2. Configuration
3. Typography & Rhythm
4. Color
5. Layout
6. Components / Patterns
7. Bonus stuff: themes, icons, animations
8. Testing
9. Documentation (?)

# What makes up a CSS framework?

## Common Patterns

* All frameworks have a sort of table of contents of sub-modules, whether it's `bootstrap.scss` or `foundation.scss`.
* Some level of basic components. You can't do much with just a grid, typography, and a reset.

## Uniformity

Establish base, *small*, almost abstract patterns:

.block {
  @include container;
  padding-top: $block-padding;
  padding-bottom: $block-padding;
}

...

.card {
  padding: 20px;
  border-bottom: 2px solid $gray;
  background: $white;
  color: $black;
}

Then configure things.

# Resets / Base Files

Most frameworks use Normalize.css. Used in both Bootstrap, Foundation, Pure, etc. Industry standard at this point. They're great at providing a foundation on which to work.

Problems with resets:

1. Normalize is heavy at ~7KB.
2. It supports a lot of browsers no one cares about.
3. It has a lot of sizing and other properties you're probably going to override.
4. Reset handles a lot of things no one cares about, i.e. weird tags
5. Resets add yet entry to the list of magical things impacting the styles of a component. They can pollute browser inspectors.

If using a third-party reset, make sure your overrides are called out. Pure does this:

https://github.com/yahoo/pure/blob/master/src/base/css/base.css

Resets can have a lot of quirk-heavy rules and be hard to follow. Nothing exceptions or modifications specifically to a reset is good. Example: `_reset.scss` and `_base.scss` with good comments.

Your reset != Default styles. They should really just be to level out the browsers. They're akin to a *standard* foundation, like how your house might be build on the same concrete blocks as my house. You don't tie in the color

Sooo... do you need a third-party reset? Do you need to reset <kbd>, <hr>, etc.

## Individual Tricks

* Using border-box to make everything match the exact sizes you specify.
*

Resources:

* http://jaydenseric.com/blog/forget-normalize-or-resets-lay-your-own-css-foundation (doesn't account for forms)

# Configuration

Most frameworks use a global file. Have always favored the global model (like Django) and wanted configuration in one spot. Downside to this is long, lengthy config files (thousands of lines). Alternatively, TOC for config:

@import "config/links";
@import "config/buttons";

Semantic UI has themes, which seem like a decent model, that override defaults.

Each theme has clear folders for overrides. You override elements specifically.

Foundation puts a lot of configuration in its related system (typography config in the `_typography.scss` file). Never liked this, typography is essential and rules from typography permeate all parts of the app. They're truly global.

However, local (file-only vars) are probably fine in a file.

Breakpoints as configuration provides a relationship between them.

Grouping similar variables in maps (dictionaries) is a good way avoid pollution. Namespacing configuration can help with this.

# Typography & Vertical Rhythm

First off - good typography is hard. Vertical rhythm is even harder.

Vertical rhythm is sizing and spacing between elements and typography that feels in sync.

*Most* CSS frameworks give you the basics. Way out of scope of this talk to get into good typography. Also hard to build a one-size-fits-all typography system.

Basic typography system in a framework is:

1. Base font size
2. Base line-height
3.


# Color

Use sensible defaults.

Color palettes

# Layout

These have gotten a lot simpler with the introduction of Flexbox. Since it's 2016, we won't be talking about old school float based grids.


# General Tips

Really, really, really avoid `!important`. SemanticUI does this:

https://github.com/Semantic-Org/Semantic-UI/blob/master/src/definitions/elements/button.less#L86

Watch your specificity:

.ui.labeled.button:not([class*="left labeled"]) > .button { .. }

Some of this can be solved with ordering in your own CSS.

Consistency in API. In Foundation, there is a `flex-order` mixin that seems like overkill, but it provides an reliably API. Sure, it's simple *now*, but maybe some bug comes up in the future? You can trust this to handle those sort of problems.

https://github.com/zurb/foundation-sites/blob/develop/scss/util/_flex.scss - Line 64

/// Changes the source order of a flex child. Children with lower numbers appear first in the layout.
/// @param {Number} $order [0] - Order number to apply.
@mixin flex-order($order: 0) {
  order: $order;
}

Note when working around browser quirks.
