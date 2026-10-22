name: bot

on:
  schedule:
    - cron: "7 * * * *"
  workflow_dispatch:

permissions:
  contents: write

concurrency:
  group: bot
  cancel-in-progress: false

jobs:
  run:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: pip install ccxt

      - name: Run bot
        run: python bot.py
        env:
          TELEGRAM_TOKEN: ${{ secrets.TELEGRAM_TOKEN }}
          CHAT_ID: ${{ secrets.CHAT_ID }}

      - name: Save state
        run: |
          git config user.name bot
          git config user.email bot@users.noreply.github.com
          git add state.json trades.csv
          git diff --staged --quiet || git commit -m update
          git pull --rebase
          git push
