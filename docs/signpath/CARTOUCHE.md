# Signing Cartouche Stream with SignPath Foundation

Cartouche Stream inherits Vibeshine's whole signing setup (see [README.md](README.md) in this
folder): two origin-verified requests, the MSI with every first-party binary and the two driver
catalogs inside it, then the setup EXE. Only the SignPath side is ours to create. Until it exists,
[cartouche-release.yml](../../.github/workflows/cartouche-release.yml) publishes unsigned builds.

Signed builds matter beyond the "unknown publisher" warning: the virtual gamepad driver only
installs from a catalog signed by `CN=SignPath Foundation`, so unsigned builds ship without it.
The driver scripts check the signer's name, not a project, so our certificate works as Vibeshine's.

## 1. Apply (the maintainer, once)

- GitHub: two-factor authentication on the account (SignPath requires it for every team member).
- Form: https://signpath.org/apply . Terms: https://signpath.org/terms .
- Answers, ready to paste:
  - **Project:** Cartouche Stream — https://github.com/Ghos1ware/cartouche-stream
  - **License:** GPL-3.0 (inherited from Vibeshine and Sunshine), no dual licensing.
  - **Description:** A Windows game-streaming host for Moonlight clients (phones, tablets, TVs). A
    fork of Vibeshine (https://github.com/Nonary/vibeshine), which SignPath Foundation already signs,
    itself a fork of Sunshine. The fork changes the product's displayed identity, a default
    (settings page local to the PC) and the update source; release branches follow Vibeshine's.
  - **Releases:** https://github.com/Ghos1ware/cartouche-stream/releases (first release 2.0.7,
    unsigned, built by GitHub Actions from this repository).
  - **Who uses it:** players of Cartouche (https://cartouche.borderline.games), a separate Windows
    game launcher that installs Cartouche Stream when the player turns streaming on and drives it
    through the server's local HTTP API. Cartouche is a separate, closed-source program: it is not
    part of this repository, not built by it, not shipped in its installer, and not signed through
    SignPath.
  - **Third-party binaries in the installer (unchanged from Vibeshine, never signed by us):**
    `nvngx_truehdr.dll` (NVIDIA RTX Video SDK runtime), SudoVDA (`SudoVDA.dll`, `sudovda.cat`,
    signed by its author), `nefconc.exe` (signed by Nefarius), usbip-win2 (Microsoft-signed
    catalogs). They are excluded from the artifact configuration, as in Vibeshine.
  - **Team:** one maintainer, Ghos1ware: committer, reviewer and approver (as Vibeshine's Nonary).
  - **Code signing policy:** README.md of the repository, section "Code signing policy".

## 2. Set up SignPath (after acceptance)

In the SignPath portal, organization given by SignPath Foundation:

1. **Project** with slug `cartouche-stream`, repository `https://github.com/Ghos1ware/cartouche-stream`.
2. **Trusted build system:** GitHub.com, and the SignPath GitHub app installed on this repository
   (origin verification needs it). The policies in `.signpath/policies/cartouche-stream/` require
   GitHub-hosted runners, which is what the release workflow uses.
3. **Artifact configurations**, pasted from the review copies in this folder:
   - slug `msi-file` ← [msi-file.artifact-config.xml](msi-file.artifact-config.xml)
   - slug `setup-exe` ← [setup-exe.artifact-config.xml](setup-exe.artifact-config.xml)
4. **Signing policies:**
   - `release-signing`: release certificate, manual approval, approver Ghos1ware.
   - `test-signing`: test certificate, for trials.
5. **CI user** submitter on both policies, and its **API token**.

On GitHub (this repository → Settings → Secrets and variables → Actions):

- variable `SIGNPATH_ORGANIZATION_ID` = the organization's ID;
- secret `SIGNPATH_API_TOKEN` = the CI user's token.

## 3. Release

Actions → "Cartouche Stream release" → Run workflow, with the version (and
`release_notes/<version>.md` committed). SignPath then shows two requests to approve: the MSI
first, then the setup. Approve each within 20 minutes, the time the signing jobs allow.
