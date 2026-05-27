# powershock-energy-market-intelligence
# PowerShock — Energy Market Intelligence Platform

## Overview
An automated agentic AI system that monitors German renewable 
energy feed-in data in real time and analyzes DAX stock volatility 
risk using live data pipelines and AI-powered analysis.

## Research Question
Does a sudden drop in renewable energy feed-in (H₁ trigger: < 20% 
of grid load) cause abnormal volatility in DAX energy-intensive 
stocks (BASF, ThyssenKrupp, Covestro, HeidelbergMaterials)?

## Tech Stack
- **n8n** — automated workflow orchestration
- **Fraunhofer ISE API** — live German energy feed-in data
- **OpenRouter AI (GPT-OSS-120B)** — agentic DAX risk analysis
- **Python** — regression, Random Forest, Granger causality
- **sktime** — time series forecasting
- **Plotly + Netlify** — interactive dashboard deployment

## Workflow Architecture
