# Hullabalook Technical Assessment

# design

Here's my design decisions for this PLP following the headings

## Lay out all products in a responsive product grid

- use grid in to render the items in a grid
  - added a basic grid style

## Mutate a computed products array

### Create a filter toggle that shows only available products

    - filter method removes any products with stock less than zero

### Create a checkbox brand filter that shows only toggled brand products

    - filter method shows only checked brand products

### Create a dropdown to sort all products into ascending or descending price order

    - select box sorts according to price or reverse
      - sort method should also reset the relevance value so the price sort can still be used after sort by relevance is set

### Add an option to sort all products by relevance - with all available products shown first in ascending rank order, then all unavailable products in ascending rank order.

    - sort by relevance button
      - method sortByRelevance
        - filter the products objects and iterate available/not

## Add a counter for the number of resulting products

- counter for length of products rendered alongside the Filters tag

Depending on the application, this could be modified to work with v-show. It might be more responsive than iterating through the whole product list every time a filter is applied.

Given more time i'd style the filters bar to scroll with the page and render in line for UX

# styling

- The component has some basic styling added inline inside the app.vue file


# Extended updates

- The brand filter has been moved to a separate component to improve the readability of the app

- The styling has been updated to:
  - align the colour scheme with the provided palette
  - allow the filter bar to scroll with the content
  - resize better to screen size
  - add a currency indicator!