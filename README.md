# svelte metagrid widget
A simple svelte component to display metagrid links in your project.

## Install

```bash
npm install @metagrid/svelte-metagrid-widget
// or
yarn add @metagrid/svelte-metagrid-widget
```

## Usage

Simply import the component into your project and use it. The component has no styles. So you have to add some css.

```sveltehtml
<script>
	import MetagridWidget from '@metagrid/svelte-metagrid-widget';
</script>

<MetagridWidget identifier="5" provider="dodis" language="en" includeDescription="true" />
```

## Props

### provider

The slug of your provider f.e. dodis

Type: String

Required: true

### identifier

The identifier of the person you want to fetch data for.

Type: String

Required: true

### langauge

The language of the widget

Type: string

Required: false

Default: de

### includeDescription

Get additional information for the provider like a description

Typ: Boolean

Required: false

Default: false

## Styling

The component has no styling. You have to theme it to your need. The following css-classes are available for theming:

```css
/* The root element of the list with the links */
ul.metagrid-list {
}
/* The li element of the link list */
li.metagrid-item {
}
/* The link itself */
a.metagrid-link {
}
/* The credit section of metagrid. Please don't remove that */
div.metagrid-credit {
}
```

## Development

If you find bugs you an open an issue or send us a pull request. We welcome contributions!

This is a svelte-kit project. To develop you can run `yarn dev`. To build the project run `yarn build`. To publish `cd package && npm publish`
