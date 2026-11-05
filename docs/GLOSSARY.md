# Glossary

| Term        | Meaning                                                                              |
| ----------- | ------------------------------------------------------------------------------------ |
| card        | A text file and an image file with the same name in `assets/`.                       |
| card name   | The file name of a card without the extension, for example `The Fool`.               |
| card text   | The Markdown text of a card.                                                         |
| deck        | All complete cards in `assets/`.                                                     |
| template    | `templates/README.template.md`. The source of `README.md`.                           |
| placeholder | An HTML comment `<!-- tarot:<name> -->` in the template that the generator replaces. |
| reading     | The card that a generated `README.md` shows, with its date.                          |
| generator   | The program in `src/` that makes `README.md`.                                        |
| daily run   | The scheduled run of the Daily README workflow at 11:00 in Amsterdam.                |
