# Your Project's Title...
Your project's description...

## Environments
- Preview: https://main--{repo}--{owner}.aem.page/
- Live: https://main--{repo}--{owner}.aem.live/

## Documentation

Before using the aem-boilerplate, we recommand you to go through the documentation on https://www.aem.live/docs/ and more specifically:
1. [Developer Tutorial](https://www.aem.live/developer/tutorial)
2. [The Anatomy of a Project](https://www.aem.live/developer/anatomy-of-a-project)
3. [Web Performance](https://www.aem.live/developer/keeping-it-100)
4. [Markup, Sections, Blocks, and Auto Blocking](https://www.aem.live/developer/markup-sections-blocks)

## Installation

```sh
npm i
```

## Linting

```sh
npm run lint
```

## Local development

1. Create a new repository based on the `aem-boilerplate` template
1. Add the [AEM Code Sync GitHub App](https://github.com/apps/aem-code-sync) to the repository
1. Install the [AEM CLI](https://github.com/adobe/helix-cli): `npm install -g @adobe/aem-cli`
1. Start AEM Proxy: `aem up` (opens your browser at `http://localhost:3000`)
1. Open the `{repo}` directory in your favorite IDE and start coding :)

## Task Management DA plugin

Both Task Management HTML entry points require the `da-lit` import map.
The plugin reads its existing `/config/placeholders.json` sheet from the
plugin's own origin to avoid cross-origin requests from DA preview to live EDS.
Preview the sheet for preview testing and publish it for live use.
Do not store credentials in this publicly served sheet.

### Comment mentions and reassignment

Opening a task's comments loads active Workfront users through the existing
`wf-actions` gateway, in pages of 100. The list is reused while the plugin is
open. A failed user lookup shows an error and a retry button; ordinary comments
can still be posted.

Use **Tag a user** to insert an `@Name` in the comment and add the user's
Workfront Note tag. Multiple users can be tagged. Removing a tag removes the
notification recipient, not the plain text; deleting the `@Name` from the
comment also excludes that recipient when posting.

Both user fields support typing a name/email or choosing from the browser's
native suggestion dropdown. Choose a complete suggested user value; incomplete
or unmatched assignee searches disable posting. Select the current assignee's
`(keep assignment)` suggestion to cancel reassignment. Native suggestion
appearance and keyboard navigation depend on the browser.

The **Assign to** dropdown initially keeps the current assignment. Selecting
a different user changes **Post** to **Assign & Post**; a nonempty comment
is required. This updates the task's primary `assignedToID`, not its complete
multi-assignee/team/role allocation. The task remains visible until the task
list is refreshed.

Reassignment is confirmed before the comment is posted. These are separate
Workfront operations, not a transaction: if assignment fails, the comment is
not sent; if assignment succeeds but posting fails, the draft is retained,
the updated assignee is retained locally, and the partial success is shown.
Check Workfront before retrying a request whose outcome was not confirmed.

The gateway must forward user-search filters/pagination, task PUT fields, and
the Note `updates` JSON parameter (including `tags` entries with `objID`,
`objObjCode: USER`, and `userID`). Workfront permissions and notification
preferences still apply. Validate real tagging and reassignment with a test
task before deployment; browser tests using mocked responses do not verify
gateway forwarding or actual notification delivery.
