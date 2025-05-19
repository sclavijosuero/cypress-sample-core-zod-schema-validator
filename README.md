# cypress-sample-core-zod-schema-validator

Sample project to use core-zod-schema-validator with Cypress.


## Installation

```sh
npm install -D core-zod-schema-validator
```


## Compatibility

- TypeScript 4.5+
- You must enable strict mode in your `tsconfig.json`. This is a best practice for all TypeScript projects.



## Usage Examples

### Example: Using `validateSchemaZod` Function in Cypress Test Framework

```js
/// <reference types="cypress" />

import { z } from "zod";
import { validateSchemaZod } from 'core-zod-schema-validator'

// Definition for the "Category" object
const categorySchema = z.object({
  id: z.number().int(),
  name: z.number().optional(), // Optional as per schema
  color: z.string()
});

// Definition for the "Tag" object
const tagSchema = z.object({
  id: z.number().int(),
  name: z.string(),
  type: z.string()
});

// Definition for the "Pet" object
const petSchema = z.object({
  id: z.string().optional(), // Optional as per schema
  age: z.number().int(),
  category: categorySchema.optional(), // Optional as per schema
  name: z.number(), // Defined as integer in the schema
  photoUrls: z.array(z.string()).min(2), // Minimum of 2 items
  tags: z.array(tagSchema).min(1).optional(), // Optional as per schema, min 1 item.
  status: z.enum(["available", "sold"]) // Specified as enum
});

const schema = z.array(petSchema);


describe('API Schema Validation Function', () => {

  const customIssuesStyles = { iconPropertyError: '⛔', iconPropertyMissing: '❓' }

  it('Test Zod Schema - Sample 2', () => {
      const findByStatusReq = {
          url: 'https://petstore.swagger.io/v2/pet/findByStatus?status=pending',
          headers: { 'Content-Type': 'application/json' }
      }

      cy.request(findByStatusReq).then((response) => {
          const result = validateSchemaZod(response.body, schema, customIssuesStyles)
          expect(result.errors).to.be.null
      });
  })
});
```
 

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.


## Changelog

### [1.0.0]
- Initial release.


## Support

If you'd like to support my work, consider buying me a coffee or contributing to a training session, so I can keep learning and sharing cool stuff with all of you. Thank you for your support!

<a href="https://www.buymeacoffee.com/sclavijosuero" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 40px !important;width: 150px !important;" ></a>
