1. Project: 
    HotDog Forecast: AI Prep Recommendation Tool

2. Target user, workflow, and business value:
    User:
    A small food vendor or hot dog stand owner operating in a high-variability environment (e.g., street stand, campus, event location).

    Workflow / Decision:
    A recurring daily decision: how many hot dogs to prepare before opening each day.

    Workflow boundaries:
        -Start: Before opening, when the owner reviews context (weather, day, recent sales...)
        -End: Decision on preparation quantity for the day

    Business value:
    Better demand prediction reduces:
        -overproduction → food waste and cost loss
        -underproduction → missed sales and customer dissatisfaction
    Improving this decision directly impacts profitability, operational efficiency, and customer experience for small vendors.

3. Problem statement and GenAI fit:
    Problem: 
        This system predicts daily hot dog demand and recommends a preparation quantity based on contextual factors such as historical sales, weather, and events.

    Why GenAI:
        The workflow involves reasoning over multiple semi-structured inputs (recent trends, qualitative signals like events, weather context). A language model can integrate these signals, recognize patterns, and produce both a quantitative estimate and an interpretable explanation.

    Why not a simple non-GenAI tool:
        A basic statistical rule (e.g., moving average) cannot:

        -incorporate qualitative inputs (e.g., “big baseball game nearby”)
        -adapt reasoning across different contexts
        -provide human-readable justification

    GenAI adds value by combining structured + unstructured signals and producing explainable decisions.

4. Planned system design and baseline
    System design / workflow:
        User inputs:
            -day of week
            -weather (temperature, rain)
            -event indicator (yes/no + description)
            -recent sales data (past 7 days)
        Pre-processing:
            -compute simple features (7-day average, trend)
        LLM call:
            structured prompt with formatted inputs
            model generates:
                -predicted demand
                -recommended prep quantity
                -confidence level
                -explanation
        Output displayed in structured format
    
    Course concepts integrated:
        Structured outputs (LLM call design):
            -The system enforces a fixed output schema (e.g., JSON with fields: prediction, recommendation, confidence, explanation) to ensure consistency and evaluability.
        Evaluation design:
            -A predefined test set of scenarios will be used to compare the system’s predictions against actual demand and against baseline methods, using quantitative error metrics.

    Baseline (comparison system):
        -7-day moving average
        -Last-week same-day sales

        These represent realistic non-AI decision strategies used by vendors.

    App description:
        A simple web app (e.g., Streamlit) where the user inputs daily context (weather, day, recent sales, events). After submission, the app displays:
            -predicted demand (number)
            -recommended preparation quantity
            -confidence level
            -short explanation
        The interface is designed for quick daily use (Prediction will be generated instantly).

5. Evaluation Plan
    What success looks like:
        The AI system provides more accurate demand predictions and better preparation recommendations than baseline methods.

    Metrics:
        Primary: Mean Absolute Error (MAE) between predicted and actual demand
        Secondary:
            -overproduction amount (waste proxy)
            -underproduction amount (lost sales proxy)
            -consistency of predictions

    Test set:
        -Synthetic dataset of ~20–30 days
        Includes variation in:
            -weekdays vs weekends
            -weather conditions
            -event vs non-event days
    Each case includes known “actual demand” for evaluation.

    Comparison vs baseline:
        For each test case:
            compute prediction error for:
                -AI system
                -7-day average
                -last-week same-day

    Then compare:
        -average MAE across all cases
        -number of cases where AI outperforms baseline

6. Example Inputs and Failure Cases

    Example inputs:

        -Sunny Friday, 75°F, baseball game nearby, high recent sales
        -Rainy Tuesday, 50°F, no events, declining recent sales
        -Saturday with festival, mixed weather, moderate sales trend
        -Monday after holiday weekend, low historical sales

    Failure cases:
        Unseen extreme events:
            -e.g., sudden road closure or unexpected crowd surge not reflected in inputs
        Poor or missing historical data:
            -e.g., incorrect past sales leads to misleading trends
        Over-reliance on weak signals:
            -e.g., model overweights “event” when actual impact is small


7. Risks and Governance

    Where the system could fail:

        -inaccurate predictions in rare or extreme conditions
        -hallucinated or misleading explanations
        -overconfidence in outputs

    Where it should not be trusted:

        -high-stakes decisions involving large inventory purchases
        -situations with missing or unreliable input data
        -unusual or one-time events not captured in the system

    Controls and human review:

        -system is decision support only, not automated execution
        -user must review recommendation before acting
        -confidence level is shown to guide trust
        -no automatic ordering or financial decisions

    Data / privacy / cost concerns:

        -use synthetic or non-sensitive data
        -minimal API usage to control cost
        -no storage of personal or sensitive information


8. Plan for the Week 6 Check-in
    By Week 6, I expect to have:
        Working app:
            -basic web interface (Streamlit)
            -functioning input → LLM → output pipeline
            -structured output format implemented

    Evaluation progress:

        -initial dataset (10–15 test cases)
        -baseline methods implemented
        -early comparison results (AI vs baseline)

    Baseline comparison:
        MAE calculated for:
            -AI system
            -7-day average
        preliminary analysis of where AI performs better or worse