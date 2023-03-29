# Assert ion-checkbox with cypress

```javascript
// cy.get('[data-cy="my-checkbox"]')はion-input
cy.get('[data-cy="my-checkbox"]').then(($checkbox) => {
  const ariaChecked = $checkbox.attr('aria-checked');
  // チェックされていることを期待するテスト
  cy.wrap(ariaChecked).should('equal', 'true');
  // チェックされていないことを期待するテスト
  cy.wrap(ariaChecked).should('equal', 'false');

});

```
