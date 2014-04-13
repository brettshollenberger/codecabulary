# Common Backbone Errors

| You see                                                                              | Real Problem                                                                                |
| --------------                                                                       | :----------------------------------------------------------------------------------------:  |
| Duplicated models when you visit a page for a second time (not on initial page load) | Your model's idAttribute is not set, or is wrong. It should be the ID used in the database. |
| Browser console error: "Can't find module 'app/collection/' or 'app/model/'          | You haven't set the id attribute of the model or collection.                                |
