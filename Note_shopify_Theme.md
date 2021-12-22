

https://new-theme-amiba.myshopify.com/admin

# Shopify Theme

### Requirement

- create a [Shopify partner account](https://www.shopify.com/partners).

-  [create a development store](https://help.shopify.com/en/partners/dashboard/development-stores) to test your theme.

  #### Steps:

  1. Log in to your [Partner Dashboard](https://partners.shopify.com/current/resources).
  2. Click **Stores**.
  3. Click **Add store**.
  4. In the **Store type** section, select **Development store**.
  5. In the **Login information** section, enter a name for your store and a password that you can use to log in. By default, the email associated with your Partner Dashboard is used as the username, but you can change that if you want.
  6. Optional: Enable a developer preview by checking **Create a non-transferrable store that uses a developer preview**. Select a developer preview version from the drop-down list.
  7. In the **Store address** section, enter your address.
  8. Optional: In the **Store purpose** section, select the reason why you're creating this development store.
  9. Click **Save**.



### Set up

1. go to shopify store

2. **Apps** -> **Create private app** 

3. fill **App details** 

4. **Admin API** -> Themes: Read and write

5. **Save**

6. **Create app**

   

   **Apps** -> **Manage private app**

   API password: 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

   store ID: new-theme-amiba.myshopify.com

### Development Tools

- ThemeKit

  https://shopify.dev/tools/theme-kit

  ```shell
  brew tap shopify/shopify
  brew install themekit
  ```

  #### Theme kit Credentials

  1. theme get --list --password=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 --store=new-theme-amiba.myshopify.com

     ==> Theme Id: 123947450538

  2. theme get -p=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 -s=new-theme-amiba.myshopify.com -t=123947450538

### Prepare environnement

```shell
//download theme files
cd desktop
mkdir new-theme-amiba
theme get -p=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 -s=new-theme-amiba.myshopify.com -t=123947450538

//watch changes
theme watch --allow-live
```

# Liquid

### Syntax

1. {{...}} denote Output
2. {%...%} denote Logic
3. Objects use the DOT syntax : shop.articles
4. Loops allow iteration over objects
5. Filters manipulate output
6. Logic controls output flow
7. Operators allow comparaison

![](https://tva1.sinaimg.cn/large/008i3skNly1grm5strc5lj30vr0dmaek.jpg)



### Object & Property

![](https://tva1.sinaimg.cn/large/008i3skNly1grm5wcswb9j30pq0bh0uy.jpg)



### handle

https://haiirboom.com/pages/contact-us

`page.handle` : contact-us

# Modify The Theme

### Create A New Section

```json
{% schema %}
    {
        "name": "Announcement bar",
        "presets": [
            {
                "name": "Announcement bar",
                "category": "ADVANCED LAYOUT"
            } 
        ],
        "settings": [
            {
                "type": "checkbox",
                "id": "show_announcement",
                "label": "Show announcement",
                "default": true
            }
        ]
    }
{% endschema %}
```

**Note:** In the above code, there is a **“presets”** under {% schema %}. **Presets** is used to Add Section in the backend “Customize” settings of the theme.

**About Presets:**

Presets must have a **name** and a **category**. Section presets and will be grouped by their category in the theme editor.

### Add the New Section as Static Section on Homepage

Just add in `theme.liquid` .

### Add Hover Action To Site Nav 

#### To Modify:

​		In `theme.js` :

```js
cache.parents.forEach(function(element) {
      // element.addEventListener('click', submenuParentClickHandler);
      element.addEventListener('mouseenter', submenuParentMouseEnterHandler);
    });
```

```js
  function showDropdown(element) {
    element.classList.add(config.activeClass);

    // if (cache.activeDropdown) hideDropdown();
    if(cache.activeDropdown) return;

    cache.activeDropdown = element;

    element
      .querySelector(selectors.siteNavLinkMain)
      .setAttribute('aria-expanded', 'true');

    setTimeout(function() {
      window.addEventListener('keyup', keyUpHandler);
      document.body.addEventListener('click', hideDropdown);
    }, 250);
  }
```

​		In `site-nav.liquid` :

```html
<button class="site-nav__link site-nav__link--main site-nav__link--button{% if link.child_active %} site-nav__link--active{% endif %}" type="button" aria-expanded="false" aria-controls="SiteNavLabel-{{ child_list_handle }}">
          <span class="site-nav__label">{{ link.title | escape }}</span>{% include 'icon-chevron-down' %}
</button> 
```

to

```html
<button class="site-nav__link site-nav__link--main site-nav__link--button{% if link.child_active %} site-nav__link--active{% endif %}" type="button" aria-expanded="false" aria-controls="SiteNavLabel-{{ child_list_handle }}">
          <a href="{{ link.url }}"
          class="site-nav__link site-nav__link--main{% if link.active %} site-nav__link--active{% endif %}"
          {% if link.current %} aria-current="page"{% endif %}
        >
          	<span class="site-nav__label">{{ link.title | escape }}</span>{% include 'icon-chevron-down' %}
          </a>
</button>
```

#### To Add:

```js
// In function cacheSelectors() line 2897
siteNavDropdown: document.querySelectorAll(selectors.siteNavDropdown)
```

```js
  function submenuParentMouseEnterHandler(event) {
    var element = event.currentTarget;

    cache.parents.forEach(function(otherElem) {
        if(otherElem != element) {
          hideDropdown();
        }
    });

		showDropdown(element);
  }
```

### Render Icon

​		snippets/icon-prompt.liquid

```
{%- render 'icon-prompt' -%}
```

### Add API

```js
var url = 'https://hook.integromat.com/8uwxy6rf2ebe22idg2sxkrb7dx226zte?';
url = url + $(this).serialize();

fetch(url)
  .then(response => {
  if(response.status != 200) {
    errorDisplay();
    document.getElementById("error_message").innerHTML = "Server is not working!";
  }
  return response.json();
})
  .then(res => {
  if(res.code == 200) {
    successDisplay();
    success.innerHTML = strTemplate.interpolate(res.data); 
  } else {
    errorDisplay();
    document.getElementById("error_message").innerHTML = "Order Number is invalid, please check again.";
  }
})
  .catch(err => console.log('Request Failed', err)); 
```

### render template with Response returned by API

​	   js:

```js
String.prototype.interpolate = function (params) {
    const names = Object.keys(params);
    const vals = Object.values(params);

    return new Function(...names, `return \`${this}\`;`)(...vals);
	};

var strTemplate = success.querySelector('template').innerHTML;
```

​		html:

```html
<template>
  <ul>
    ${checkpoints.map(function (obj, index) {
      var date = new Date(`${obj.checkpoint_time}`).toDateString();
      var time = new Date(`${obj.checkpoint_time}`).toLocaleTimeString('en-US', {hour: '2-digit', minute: '2-digit'});
      return `
        <li>
          <div>${date} ${time}</div>
          <div>${obj.message}</div>
          <div>${obj.location}</div>
        </li>
      `;
    }).reverse().join('')}
  </ul>
</template>
```

### Import independent css/js

```handlebars
 {% if page.handle == 'shipping-tracking' %}
  <script src="https://code.jquery.com/jquery-3.5.1.min.js">		</script>
  {{ 'shipping-tracking.css' | asset_url | stylesheet_tag }}
 {% endif %}
 {% if template.name =='product' %}
   {{ 'popups.css' | asset_url | stylesheet_tag }} 
 {% endif %}
```



