# Cyberpunk 2077 Tarot: card of the day

Night City has a message for you. Each day at 11:00 (Amsterdam time), a GitHub Actions workflow draws one card from the Cyberpunk 2077 tarot deck and puts it on this page.

## Reading for <!-- tarot:date -->

<p align="center">
  <!-- tarot:image -->
</p>

### <!-- tarot:name -->

<!-- tarot:text -->

---

## How it works

- `npm run readme` selects one random card from [`assets/`](assets) and writes this `README.md` from [`templates/README.template.md`](templates/README.template.md).
- [The daily workflow](.github/workflows/daily-readme.yml) runs the command and pushes the new `README.md` to `master`.
- Do not edit `README.md`. Edit the template. See [the generator documentation](docs/GENERATOR.md) and [the setup guide](docs/SETUP.md).

Cyberpunk 2077 is a trademark of CD PROJEKT S.A. This is a non-commercial fan project.
