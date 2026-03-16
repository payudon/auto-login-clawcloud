# 文件名: .github/workflows/clawcloud-login.yml
name: ClawCloud Run Auto Login (2FA Support)

on:
  schedule:
    - cron: '0 0 */15 * *'
  workflow_dispatch:

jobs:
  auto-login:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.9'

      # 【终极杀招 1：安装虚拟显示器环境】
      - name: Install System Dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y xvfb

      - name: Install Python Dependencies
        run: |
          python -m pip install --upgrade pip
          # 【新增】安装 playwright-stealth 隐身插件
          pip install playwright pyotp playwright-stealth
          playwright install chromium

      - name: Run Login Script
        env:
          GH_USERNAME: ${{ secrets.GH_USERNAME }}
          GH_PASSWORD: ${{ secrets.GH_PASSWORD }}
          GH_2FA_SECRET: ${{ secrets.GH_2FA_SECRET }}
        # 【终极杀招 2：用 xvfb-run 模拟真实桌面环境运行 Python，强制带上 -u 参数输出日志】
        run: xvfb-run --auto-servernum --server-args="-screen 0 1920x1080x24" python -u login_script.py

      - name: Upload Debug Screenshots
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: all-debug-screenshots
          path: "*.png"
