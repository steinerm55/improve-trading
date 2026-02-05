# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository explores extensibility and automation for two trading platforms:

- **MetaTrader 4 (MT4)** — Desktop platform, extended via MQL4 (C++-like) with Expert Advisors, custom indicators, scripts, and DLL imports
- **TradingView** — Web platform, extended via Pine Script (v6) with indicators, strategies, libraries, and webhook-based alerts

## Domain Context

- MT4 extensions use `.mq4` source files compiled to `.ex4` via MetaEditor
- TradingView Pine Script code lives on TradingView's servers (no local compilation)
- MT4 can execute trades directly from code; TradingView requires webhooks + external services for live execution
- MT4 supports DLL imports for external language integration (C/C++/C#/Python); Pine Script is fully sandboxed

## Repository

- Remote: https://github.com/steinerm55/improve-trading.git
- Branch: main
