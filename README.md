# Bygga workflows med GitHub Actions


## 📚 Innehållsförteckning
1. [Introduktion](#introduktion)
2. [Hur fungerar det?](#hur-fungerar-det)
3. [Workflow-fil (YAML)](#workflow-fil-yaml)
4. [Hur man använder workflowet](#hur-man-använder-workflowet)

---

## 📖 Introduktion
När man arbetar med ett projekt i GitHub är det viktigt att validera koden automatiskt för att undvika att felaktig kod hamnar i `main`. Ett **build workflow** säkerställer att:

- Koden kompileras korrekt.
- Alla beroenden är korrekta.
- En fungerande kod upprätthålls i hela projektet.

---


## ⚙️ Hur fungerar det?
Workflowet använder GitHub Actions för att köra en rad steg (steps) varje gång en förändring görs i projektet. 

---

## 📝 Workflow-fil (YAML)

I detta exempel används följande YAML-fil som används för att skapa ett enkelt build-workflow. 

```yaml
name: Build Project   # name: namnet på workflow

on:
# on: här definieras de triggers (händelser) som ska starta workflowet
  push:
    branches:
      - main
 # Kör workflowet vid push till main
  pull_request:  
    branches:
      - main
# Kör även workflowet vid pull requests mot main

jobs:
 #    Ett workflow kan innehålla ett eller flera jobs.
 #    Varje job är en sekvens av steg (steps) som körs på en specifik miljö.

  build:   # Detta är namnet på detta job, dvs 'build'
    runs-on: ubuntu-latest

  # runs-on används eftersom en virtuell maskin ska köra jobbet
  # När workflowet startar (t.ex. efter en push till main), tillhandahåller GitHub Actions
  #  en ny virtuell maskin (VM) baserad på det operativsystem du valt
  # (i det här fallet ubuntu-latest).

    steps:
    - name: Checkout code
      uses: actions/checkout@v2  # Klona koden från repository

    - name: Setup .NET
      uses: actions/setup-dotnet@v3  # Installera .NET SDK
      with:
        dotnet-version: '7.0'  # Anpassa till din .NET-version

    - name: Restore dependencies
      run: dotnet restore  # Installera NuGet-beroenden

    - name: Build solution
      run: dotnet build --configuration Release  # Bygg projektet
