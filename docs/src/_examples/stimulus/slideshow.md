---
top_section: Stimulus
title: Slideshow
order: 15
category: slideshow
---

# Configure when necessary

Add the following to your `public/index.html`:

```html
<div data-controller="slideshow">
  <button data-action="slideshow#previous"> ← </button>
  <button data-action="slideshow#next"> → </button>

  <div data-slideshow-target="slide">🐵</div>
  <div data-slideshow-target="slide">🙈</div>
  <div data-slideshow-target="slide">🙉</div>
  <div data-slideshow-target="slide">🙊</div>
</div>
```

Now create a `src/controllers/slideshow_controller.js.rb` file with the following
contents:

<div data-controller="combo" data-options='{
  "eslevel": 2022,
  "autoexports": "default",
  "filters": ["esm", "functions", "stimulus"]
}'></div>

```ruby
class SlideshowController < Stimulus::Controller
  self.values = { index: Number }

  def next()
    indexValue += 1
  end

  def previous()
    indexValue -= 1
  end

  def indexValueChanged()
    showCurrentSlide()
  end

  def showCurrentSlide()
    slideTargets.each_with_index do |element, index|
      element.hidden = index != indexValue
    end
  end
end
```

### Results

<p data-controller="eval" data-html=".language-html"></p>

Click the arrow buttons, and see the slides change.

# Commentary

New here is the `each_with_index` passing a block, but what is more notable is
the `self.targets` at the top.  Perhaps you are thinking, *I was told there
would be no configuration*, but that's not quite true.  What convention over
configuration is all about is providing reasonable defaults.  And given the
set of [Value Types](https://stimulus.hotwire.dev/reference/values#types) that
Stimulus supports, the only reasonable default is `String`.  Should you want
something different, all it takes is one line to tell the Stimulus filter what
you want.

Oh, and by the way, feel free to add `% slideTargets.length` to the statement
inside the `each_with_index` call to cause the slideshow to wrap around.
