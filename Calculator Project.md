describe('Basic Calculator Functionality test', () => {
    beforeEach(() => {
      cy.visit('https://testsheepnz.github.io/BasicCalculator')
      // Select which build to test
      cy.get('[data-testid="selectBuild"]').select(2)
    })
    it('All elements are visible and active', () => {
      // Dropdowns
      cy.get('[data-testid="selectBuild"]').should('be.visible');
      cy.get('[data-testid="selectOperationDropdown"]').should('be.visible');
  
      // Input fields
      cy.get('[data-testid="number1Field"]').should('be.visible');
      cy.get('[data-testid="number2Field"]').should('be.visible');
  
      // Buttons
      cy.get('[data-testid="calculateButton"]')
        .should('be.visible')
        .and('not.be.disabled');
      cy.get('[data-testid="clearButton"]')
        .should('be.visible')
        .and('not.be.disabled');
  
      // Answer field and integers only checkbox
      cy.get('[data-testid="numberAnswerField"]').should('be.visible');
      cy.get('[data-testid="integerSelect"]')
        .should('be.visible')
        .and('not.be.checked');
    });
  
    it('Add 2 + 5 = 7', () => {
      cy.get('[data-testid="number1Field"]').type(2)
      cy.get('[data-testid="number2Field"]').type(5)
      cy.get('[data-testid="selectOperationDropdown"]').select('Add')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 7)
    })
    it('Add 2.2 + 5.1 = 7.3', () => {
      cy.get('[data-testid="number1Field"]').type(2.2)
      cy.get('[data-testid="number2Field"]').type(5.1)
      cy.get('[data-testid="selectOperationDropdown"]').select('Add')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 7.3)
    })
    it('Add 2.2 + 5.1 + integers only checkbox = 7', () => {
      cy.get('[data-testid="number1Field"]').type(2.2)
      cy.get('[data-testid="number2Field"]').type(5.1)
      cy.get('[data-testid="selectOperationDropdown"]').select('Add')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="integerSelect"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 7)
    })
    it('Add a + 2 = Error Number 1 is not a number', () => {
      cy.get('[data-testid="number1Field"]').type('a')
      cy.get('[data-testid="number2Field"]').type(2)
      cy.get('[data-testid="selectOperationDropdown"]').select('Add')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="errorMsgField"]')
        .should('be.visible')
        .and('have.text', 'Number 1 is not a number')
    })
    it('Add 7 - b = Error Number 2 is not a number', () => {
      cy.get('[data-testid="number1Field"]').type(7)
      cy.get('[data-testid="number2Field"]').type('b')
      cy.get('[data-testid="selectOperationDropdown"]').select('Subtract')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="errorMsgField"]')
        .should('be.visible')
        .and('have.text', 'Number 2 is not a number')
    })
    it('Subtract 22 - 30 = -8', () => {
      cy.get('[data-testid="number1Field"]').type(22)
      cy.get('[data-testid="number2Field"]').type(30)
      cy.get('[data-testid="selectOperationDropdown"]').select('Subtract')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', -8)
    })
    it('Multiply 7 * 77777 = 544 439', () => {
      cy.get('[data-testid="number1Field"]').type(7)
      cy.get('[data-testid="number2Field"]').type(77777)
      cy.get('[data-testid="selectOperationDropdown"]').select('Multiply')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 544439)
    })
    it('Multiply 351 * 0 = 0', () => {
      cy.get('[data-testid="number1Field"]').type(351)
      cy.get('[data-testid="number2Field"]').type(0)
      cy.get('[data-testid="selectOperationDropdown"]').select('Multiply')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 0)
    })
    it('Divide 99 / 33 = 3', () => {
      cy.get('[data-testid="number1Field"]').type(99)
      cy.get('[data-testid="number2Field"]').type(33)
      cy.get('[data-testid="selectOperationDropdown"]').select('Divide')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 3)
    })
    it('Divide 123 / 0 = Error cannot divide by zero', () => {
      cy.get('[data-testid="number1Field"]').type(351)
      cy.get('[data-testid="number2Field"]').type(0)
      cy.get('[data-testid="selectOperationDropdown"]').select('Divide')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="errorMsgField"]')
      .should('be.visible')
      .and('have.text', 'Divide by zero error!')
    })
    it('Concatenate a + bc = abc', () => {
      cy.get('[data-testid="number1Field"]').type('a')
      cy.get('[data-testid="number2Field"]').type('bc')
      cy.get('[data-testid="selectOperationDropdown"]').select('Concatenate')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 'abc')
    })
    it('Concatenate Test + 998 = Test998', () => {
      cy.get('[data-testid="number1Field"]').type('Test')
      cy.get('[data-testid="number2Field"]').type(998)
      cy.get('[data-testid="selectOperationDropdown"]').select('Concatenate')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 'Test998')
    })
    it('Concatenate 100 + 200 = 100200', () => {
      cy.get('[data-testid="number1Field"]').type(100)
      cy.get('[data-testid="number2Field"]').type(200)
      cy.get('[data-testid="selectOperationDropdown"]').select('Concatenate')
      cy.get('[data-testid="calculateButton"]').click()
      cy.get('[data-testid="numberAnswerField"]').should('have.value', 100200)
    })
  })
