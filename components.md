# Componentes

## App

- Define las opciones de la navegación principal de la aplicación
- Renderiza el componentes Layout
- Proyecta en el anterior el componente App-Router

## App-Router

- Define las rutas de la aplicación, cada una asociada a un componente
- Renderiza el html main que contendrá el/los elementos de cada ruta (página)
- Renderiza el componente principal de la ruta activa

## Layout

- Renderiza los componentes Header y Footer
- Proyecta en al Header el componente Menu
- Recibe las opciones por props desde App y las pasa a Menu
- Actúa de wrapper del componente App-Router

## Header

- Renderiza el título de la aplicación
- Renderiza el componente Menu, que recibe como proyección de contenido

## Footer

- Renderiza el componente Footer

## Menu (Lista (Home) ## Favoritos)

- Renderiza las opciones del menu
- Recibe las opciones por props desde Layout
- Interacciones: cuando el usuario hace click
  - navega declarativamente a la ruta correspondiente a la opción del menú seleccionada

## Context-Provider

- Ejecuta el hook que define el estado de la aplicación
  - Repo:
    - Load de los items (página 1)
    - Load de los favoritos
- Proporciona acceso via Contexto a los elementos del estado

**Estado**:

- items: Item[]
- itemsPage: number
- favorites: Item[]
- selectedItem: Item

## Items-Lista (API pública) -> PAGE

- Itera la lista de items que recoge del contexto. En cada iteración:
  - Renderiza un componente Card pasándole por props el item correspondiente
- Renderiza el componente Pagination-buttons

## Card

- Renderiza el item que recibe por props.
- Interacciones: cuando el usuario hace click
  - actualiza el valor selectedItem del contexto con el item actual
  - navega declarativamente a la ruta 'details'

## Pagination-buttons

- Renderiza los botones anterior/siguiente

## Favorites-Lista (API privada) -> PAGE

- Itera la lista de favoritos que recoge del contexto. En cada iteración:
  - Renderiza un componente Card pasándole por props el item correspondiente

## Create-item -> PAGE

## Item-Form (create/update)

## Modify-item -> PAGE

## Detail -> PAGE

## Not-Found -> PAGE
