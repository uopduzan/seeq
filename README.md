# seeq

Below is a reconstructed technical scope from your meeting notes, followed by the meaning of each request and the specific work needed in Seeq.

1. Meeting objective

Develop batch-level Seeq analyses for the LABIX Linear Alkyl Benzene complex to:

Compare plant performance between batches and operating days.
Quantify relationships among paraffin consumption, LAB production, conversion, and catalyst or unit life.
Replace daily aggregated results with batch-level results where possible.
Determine whether conversion can predict cumulative production or remaining operating life.
Quantify variability and uncertainty around observed trends.
2. Technical breakdown
A. Paraffin consumption versus single-pass conversion
Requested analysis

Create a scatter plot with:

X-axis: Single-pass conversion, %
Y-axis: Paraffin mass consumption per batch, mass units
One point: One completed batch
Trend: Linear regression

Add statistical limits around the regression:

±1 standard deviation
95% confidence interval
What it means

The purpose is to determine whether paraffin consumption changes predictably with conversion and to quantify normal variability around that relationship.

Your note, “Find 1st standard deviation, 95% confidence interval,” likely refers to two separate statistical bands:

±1σ band: Indicates the typical spread of batch results around the fitted relationship.
95% confidence interval: Indicates uncertainty in the estimated mean trend.

For evaluating whether an individual future batch is abnormal, a 95% prediction interval is more useful than a confidence interval. Ideally, show:

Regression line
±1 residual standard deviation
95% prediction interval
Required calculations

For batch ii:

Conversioni=Paraffin Feedi−Unconverted ParaffiniParaffin Feedi×100\text{Conversion}_i = \frac{\text{Paraffin Feed}_i-\text{Unconverted Paraffin}_i} {\text{Paraffin Feed}_i}\times100 Paraffin Consumptioni=∫Batch StartBatch EndParaffin Mass Flow dt\text{Paraffin Consumption}_i = \int_{\text{Batch Start}}^{\text{Batch End}} \text{Paraffin Mass Flow}\,dt

Fit:

Paraffin Consumptioni=β0+β1Conversioni+ϵi\text{Paraffin Consumption}_i = \beta_0+\beta_1\text{Conversion}_i+\epsilon_i

Report:

Regression slope and intercept
R2R^2
Number of batches
Residual standard deviation
95% confidence interval for the mean trend
95% prediction interval for individual batches
B. Conversion versus total LAB produced per batch
Requested analysis

Build a batch-level table and plot showing the relationship between:

Single-pass conversion
Total LAB produced during the batch
Recommended table
Batch	Start time	End time	Duration	Conversion	Total LAB produced	Average LAB rate	Paraffin consumedBatch ID	Timestamp	Timestamp	Hours	%	Mass	Mass/time	Mass
Recommended plot
X-axis: Single-pass conversion, %
Y-axis: Total LAB produced per batch
One point: One batch
Color points by operating date, catalyst cycle, train, or operating mode if applicable.
Add a regression line only if the relationship is approximately linear.
What it means

This analysis tests whether higher conversion corresponds to higher total LAB production.

Batch duration must be included because total LAB production is affected by both:

Production rate
Length of the batch

A longer batch can produce more LAB even if conversion is lower. Therefore, review both:

Total LAB produced per batch
Average LAB production rate per batch
Total LABi=∫Batch StartBatch EndLAB Production Rate dt\text{Total LAB}_i = \int_{\text{Batch Start}}^{\text{Batch End}} \text{LAB Production Rate}\,dt Average LAB Ratei=Total LABiBatch Durationi\text{Average LAB Rate}_i = \frac{\text{Total LAB}_i}{\text{Batch Duration}_i}
C. Conversion versus paraffin feed consumption
Requested analysis

Build a batch-level table and scatter plot showing:

X-axis: Paraffin feed consumption per batch
Y-axis: Single-pass conversion
What it means

This analysis determines whether batches consuming more paraffin operate at a different conversion.

There is an important distinction between:

Total paraffin charged or consumed per batch
Average paraffin feed rate
Specific paraffin consumption per unit LAB

All three should be calculated because total consumption is strongly influenced by batch duration.

Specific Paraffin Consumptioni=Paraffin ConsumptioniTotal LAB Producedi\text{Specific Paraffin Consumption}_i = \frac{\text{Paraffin Consumption}_i} {\text{Total LAB Produced}_i}

The specific-consumption KPI is likely the most useful efficiency measure because it normalizes paraffin consumption by production.

D. Replace daily aggregates with per-batch trends
Requested analysis

Change the current trend from one value per calendar day to one value per batch.

Use conversion as a primary batch KPI.

What “use conversion” most likely means

Conversion should be used as the main performance variable when comparing batches. It may be intended as:

The primary Y-axis KPI in a batch sequence trend.
A predictor for LAB production or feed consumption.
A variable for comparing performance across catalyst life.
A filter for identifying batches operated near a target, such as 12.5%.
Recommended batch trend
X-axis: Batch number or batch start timestamp
Y-axis: Batch-average conversion
Secondary variables:
Total LAB produced
Paraffin consumption
Specific paraffin consumption
Batch duration
Average LAB rate

Use one result per batch rather than averaging all data into calendar-day capsules.

Why this matters

A daily aggregate can combine portions of multiple batches or divide one batch across two days. That can obscure:

Start-to-end batch behavior
Batch-to-batch variability
Short abnormal batches
Changes in operating conditions between batches
Relationships between conversion and batch production
E. Conversion versus catalyst or operating life
Requested analysis

Develop a relationship between conversion and “life,” where life is represented by cumulative LAB production.

The intended business question appears to be:

If LABIX is operated at 12.5% conversion, how much cumulative LAB production, or operating life, should be expected within approximately ±5% accuracy?

Define the life variable

For each catalyst or operating cycle:

Cumulative LAB Lifei=∑j=1iTotal LAB Producedj\text{Cumulative LAB Life}_i = \sum_{j=1}^{i}\text{Total LAB Produced}_j

Plot:

X-axis: Cumulative LAB production since cycle start
Y-axis: Batch conversion

This shows conversion degradation as production accumulates.

Suitable model forms

Start with three candidate models:

Linear degradation

C=β0+β1LC = \beta_0+\beta_1L

Exponential degradation

C=C0e−kLC = C_0e^{-kL}

Quadratic or nonlinear empirical model

C=β0+β1L+β2L2C = \beta_0+\beta_1L+\beta_2L^2

Where:

CC = conversion
LL = cumulative LAB production

Select the model using:

Residual behavior
Cross-validated prediction error
R2R^2, used as supporting information rather than the sole selection criterion
Physical reasonableness
Performance on held-out batches or catalyst cycles
Predicting life at 12.5% conversion

After fitting the selected model, solve it for the cumulative LAB production corresponding to:

C=12.5%C=12.5\%

The output should be reported as:

Estimated cumulative LAB production at 12.5% conversion = LpredictedL_{\text{predicted}}, with a 95% prediction interval of LlowL_{\text{low}} to LhighL_{\text{high}}.

Important clarification about ±5%

“±5% accuracy” could mean either:

Relative life accuracy: Predicted life is within ±5% of actual cumulative LAB production.
Conversion tolerance: Conversion is within ±5% of 12.5%, equivalent to approximately 11.875% to 13.125%.
Absolute conversion tolerance: 12.5% ± 5 percentage points, equivalent to 7.5% to 17.5%.

The first interpretation is the most likely. It should be confirmed before claiming the model meets the requirement.

Do not report ±5% predictive accuracy unless evaluation against held-out data demonstrates it.

F. Batch-level versus 40-day variability
Requested analysis

Compare short-term batch variability with variability observed over a rolling 40-day period.

What it means

The meeting observation was likely that individual batches have substantial variation, but the variation becomes smoother when results are aggregated over approximately 40 days.

Calculate:

Batch-level mean and standard deviation
Daily mean and standard deviation
Rolling 40-day mean
Rolling 40-day standard deviation
Coefficient of variation
CV=σμ×100%CV=\frac{\sigma}{\mu}\times100\%
Recommended visualization

Use a batch-sequence trend containing:

Individual batch conversion values
Rolling mean
Rolling ±1σ limits
Optional 95% control or prediction limits

Do not substitute a 40-day aggregate for the batch data. Display the smoothed trend on top of the individual results so both short-term variation and long-term degradation remain visible.

3. Required Seeq data structure
Batch capsule definition

Create or validate a batch capsule for every operating batch using:

Batch start condition
Batch end condition
Batch ID
Valid production state
Catalyst or operating cycle ID

Exclude or separately classify:

Startup and shutdown periods
Incomplete batches
Trips
Missing-flow periods
Instrument bad-quality periods
Maintenance or circulation-only operation
Batches with material-balance failure
Batch-level KPIs

For every valid batch, calculate:

Batch start and end
Batch duration
Single-pass conversion
Total paraffin feed
Total paraffin consumption
Total LAB production
Average LAB production rate
Specific paraffin consumption
Cumulative LAB production
Catalyst or operating-cycle age
Data-quality status
Operating mode or train, if relevant
4. Deliverables
Deliverable 1: Batch KPI table

One row per batch with all calculated KPIs and data-quality flags.

Deliverable 2: Paraffin consumption versus conversion

Scatter plot with:

Linear regression
R2R^2
±1 residual standard deviation
95% prediction interval
Deliverable 3: Conversion versus LAB production

Two plots:

Conversion versus total LAB per batch
Conversion versus average LAB production rate
Deliverable 4: Conversion versus paraffin feed

Three plots:

Conversion versus total paraffin feed per batch
Conversion versus average paraffin feed rate
Conversion versus specific paraffin consumption
Deliverable 5: Batch conversion trend

Batch-by-batch conversion with:

Rolling average
Rolling standard deviation
Catalyst cycle or cumulative production reference
Deliverable 6: Conversion-life model

Model conversion as a function of cumulative LAB production and estimate the cumulative production associated with 12.5% conversion.

Include:

Selected model equation
Prediction interval
Validation error
Actual-versus-predicted results
Determination of whether ±5% life accuracy is achievable
Deliverable 7: Variability comparison

Compare:

Individual-batch variability
Daily aggregated variability
Rolling 40-day variability
5. Action items
Priority 1: Establish the batch basis
 Identify the authoritative Seeq batch start and end logic.
 Create one capsule per LABIX batch.
 Assign batch ID and catalyst or operating-cycle ID.
 Define exclusion rules for trips, startup, shutdown, and incomplete batches.
 Confirm the time range and number of cycles to analyze.
Priority 2: Confirm KPI definitions
 Confirm the exact single-pass conversion calculation.
 Confirm which signal represents total paraffin feed.
 Confirm whether “paraffin consumption” means gross feed, net consumption, or material-balance consumption.
 Confirm which LAB flow or inventory signal represents production.
 Define cumulative LAB production from the start of each catalyst cycle.
 Confirm mass units and reconcile unit conversions.
Priority 3: Build the batch table
 Calculate one value per KPI per batch.
 Add batch duration and production-rate normalization.
 Add data-quality and operating-status columns.
 Review outliers before performing regression.
Priority 4: Build the relationship plots
 Plot paraffin consumption versus conversion.
 Plot conversion versus total LAB per batch.
 Plot conversion versus average LAB rate.
 Plot conversion versus total paraffin feed.
 Plot conversion versus specific paraffin consumption.
 Add regression statistics and uncertainty bands.
Priority 5: Build the life model
 Plot conversion against cumulative LAB production.
 Separate catalyst cycles rather than treating them as one continuous series.
 Fit linear, exponential, and nonlinear candidate models.
 Validate using held-out batches or complete held-out cycles.
 Calculate predicted life at 12.5% conversion.
 Report uncertainty and assess whether ±5% accuracy is supported.
Priority 6: Compare time resolutions
 Preserve the individual batch results.
 Calculate daily aggregates for comparison only.
 Calculate rolling 40-day statistics.
 Determine whether the 40-day window masks meaningful batch-level abnormalities.
6. Questions requiring confirmation
What defines the start and end of a LABIX batch?
What is the approved formula for single-pass conversion?
Does “paraffin consumption” mean total feed or net reacted paraffin?
Is plant life defined by cumulative LAB production, elapsed days, or both?
Does life reset when catalyst is replaced or regenerated?
Is 12.5% conversion a target, an end-of-life threshold, or an example operating point?
Does ±5% apply to predicted life, conversion, or the model confidence range?
Should abnormal batches be removed, flagged, or modeled separately?
Should the 40-day comparison be a rolling window or fixed 40-day operating periods?
Are different trains, feed compositions, catalyst lots, or operating modes expected to be modeled separately?
Recommended final interpretation

The primary assignment is to create a batch-centered performance dataset in Seeq. Conversion is the main response variable, and paraffin consumption, LAB production, and cumulative LAB production are the main explanatory variables. The final analysis should describe batch-to-batch variability, distinguish it from the smoother 40-day trend, and determine whether conversion can reliably predict catalyst or unit life.
