Rights Reserved, Unlicensed
# GPG Signed Commits — Quickstart (macOS, zsh)

0) Prereqs
  brew install pinentry-mac
  mkdir -p ~/.gnupg
  chmod 700 ~/.gnupg
  echo "pinentry-program /opt/homebrew/bin/pinentry-mac" > ~/.gnupg/gpg-agent.conf
  grep -q 'GPG_TTY=' ~/.zshrc || echo 'export GPG_TTY=$(tty)' >> ~/.zshrc
  gpgconf --kill gpg-agent
  exec /bin/zsh -l

1) Generate key
  gpg --quick-generate-key "Dr. Meg Montañez-Davenport <dr.meg.data.scientist@gmail.com>" rsa4096 sign,cert 0

2) Find key ID
  gpg --list-secret-keys --keyid-format=long

3) Configure git for this repo
  cd ~/mydata-did-consent
  git config user.name "Dr. Meg Montañez-Davenport"
  git config user.email "dr.meg.data.scientist@gmail.com"
  KEY_ID=$(gpg --list-secret-keys --keyid-format=long | awk '/sec/{print $2}' | awk -F'/' 'NR==1{print $2}')
  git config commit.gpgsign true
  git config gpg.program gpg
  git config user.signingkey "$KEY_ID"

4) Upload key to GitHub
  gpg --armor --export "$KEY_ID" | pbcopy
  open "https://github.com/settings/gpg/new"

5) Make a signed test commit
  echo "GPG signing enabled $(date -u +"%Y-%m-%dT%H:%M:%SZ")" >> docs/GPG-SIGNING-QUICKSTART.md
  git add docs/GPG-SIGNING-QUICKSTART.md
  git commit -S -m "docs: add GPG signing quickstart and verify signed commit"

6) Push branch and open PR
  git fetch origin
  git checkout -b docs/gpg-signing-quickstart
  git push --set-upstream origin docs/gpg-signing-quickstart
  open "$(git remote get-url origin | sed -E 's/.git$//' )/compare/docs/gpg-signing-quickstart?expand=1"

Troubleshooting
  gpgconf --kill gpg-agent
  grep -n 'pinentry-program' ~/.gnupg/gpg-agent.conf || echo "pinentry-program /opt/homebrew/bin/pinentry-mac"
  git config user.email
