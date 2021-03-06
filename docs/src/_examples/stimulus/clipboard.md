---
top_section: Stimulus
title: Clipboard
order: 14
category: clipboard
---

# Seamless integration with JavaScript

Place the following into a file named `public/clipboard.css`:

```css
.clipboard-button {
  display: none;
}

.clipboard--supported .clipboard-button {
  display: initial;
}
```

Add the following to your `public/index.html`:

```html
<div data-controller="clipboard">
  PIN: <input data-clipboard-target="source" type="text" value="1234" readonly>
  <button data-action="clipboard#copy" class="clipboard-button">Copy to Clipboard</button>
</div>

<div data-controller="clipboard">
  PIN: <input data-clipboard-target="source" type="text" value="3737" readonly>
  <button data-action="clipboard#copy" class="clipboard-button">Copy to Clipboard</button>
</div>
```

Now create a `src/controllers/clipboard_controller.js.rb` file with the following
contents:

<div data-controller="combo" data-options='{
  "eslevel": 2022,
  "autoexports": "default",
  "filters": ["esm", "stimulus"]
}'></div>

```ruby
import "../clipboard.css"

class ClipboardController < Stimulus::Controller
  def connect()
    if document.queryCommandSupported("copy")
      element.classList.add("clipboard--supported")
    end
  end

  def copy(event)
    event.preventDefault()
    sourceTarget.select()
    document.execCommand("copy")
  end
end
```

### Results

<p data-controller="eval" data-html=".language-html" data-css=".language-css"></p>

Click a copy button, and see it in action.  Paste the results elsewhere.

# Commentary

What is notable here is the seamless access to JavaScript functionality.
Note the `import` at the top of the file as well as the calls to methods on the
`document`, `element`, `event`, and `sourceTarget` objects.  The syntax for
these calls is identical to what you would code in JavaScript.

Feel free to go wild.  Just be aware that seemless integration with JavaScript
is an example of the [provide sharp
knives](https://rubyonrails.org/doctrine/#provide-sharp-knives) doctrine.

Also note that Ruby2JS's Stimulus
filter knows to only add `this.` to the `element` and `sourceTarget`
references.  The `document`, and `event` objects are left alone.
