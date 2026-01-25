# JMeter Performance Test Plan Template

This is a template/reference for creating your JMeter Test Plan.

## ⚠️ Important

This is NOT a `.jmx` file. You need to create the actual JMeter Test Plan using the JMeter GUI application.

## How to Create Your Test Plan

1. **Follow the SETUP_GUIDE.md** - It has step-by-step instructions
2. **Use JMeter GUI** to create the test plan
3. **Save your test plan** as `performance-test.jmx` in this directory

## Test Plan Structure

Your JMeter test plan should have the following structure:

```
Test Plan: Performance Test - [Website Name]
│
├── HTTP Request Defaults
│   ├── Server: [domain]
│   ├── Protocol: https
│   └── Port: 443
│
├── Thread Group 1 - Basic Scenario
│   ├── Threads: 10
│   ├── Ramp-up: 1s
│   ├── Loop: 5
│   └── HTTP Request: Homepage (GET /)
│
├── Thread Group 2 - Heavy Load
│   ├── Threads: 50
│   ├── Ramp-up: 30s
│   ├── Loop: 1
│   ├── HTTP Request: Homepage (GET /)
│   └── HTTP Request: Subpage (GET /[path])
│
├── Thread Group 3 - Custom Scenario
│   ├── Threads: 20
│   ├── Ramp-up: 10s
│   ├── Duration: 60s
│   ├── Loop: Forever
│   ├── HTTP Request: [Option A] POST or [Option B] GET /page1
│   └── HTTP Request: [Option A] GET or [Option B] GET /page2
│
└── Listeners
    ├── View Results Tree
    ├── Summary Report
    └── Aggregate Report
```

## After Creating Your Test Plan

1. Save it as `performance-test.jmx`
2. The file should be in: `automation_test/jmeter/performance-test.jmx`
3. Commit it to your repository
4. You can open and modify it later by: **File → Open** in JMeter

## Quick Start

```bash
# Navigate to JMeter bin folder
cd C:\JMeter\apache-jmeter-5.6.3\bin

# Launch JMeter GUI
jmeter.bat

# Or on Linux/Mac:
./jmeter.sh
```

Then follow SETUP_GUIDE.md to create your test plan step by step.

---

**Note:** The `.jmx` file is XML-based and can be edited manually, but it's recommended to use the JMeter GUI for easier configuration.
