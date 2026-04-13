<svelte:head>
  <title>Resume</title>
</svelte:head>

<h1>Resume</h1>

<section>
  <h2>Education</h2>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">Massachusetts Institute of Technology, Sloan School of Management</span>
      <span class="entry-location">Cambridge, MA</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Candidate for Master of Business Analytics, Operations Research Center</span>
      <span class="entry-date"><time datetime="2025">2025</time> – <time datetime="2026-08">August 2026</time> | GPA: 5.0/5.0</span>
    </div>
    <ul>
      <li>Courses: Machine Learning, Optimization, Analytics Lab, Analytics Edge, Deep Learning, Climate and Energy in the Global Economy</li>
      <li>ML Project: Produced three price forecasts for the Texas grid and used them to drive an optimization model simulating generator operation for downstream comparison (Python, Julia)</li>
      <li>Optimization Project: Created a dynamic programming model to optimally place Catan board game settlements (Python)</li>
    </ul>
  </div>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">Tulane University, School of Science and Engineering</span>
      <span class="entry-location">New Orleans, LA</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Bachelor of Science, Majors: Mathematics & Economics</span>
      <span class="entry-date"><time datetime="2019">2019</time> – <time datetime="2023">2023</time></span>
    </div>
    <ul>
      <li>Sole recipient of the J. Ernest Tanner Award for Excellence in Economics, as voted on by the department</li>
      <li>Tulane Honors Scholar Recipient ($30k annually) and member of dean's list for all semesters</li>
    </ul>
  </div>
</section>

<section>
  <h2>Experience</h2>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">MIT Sloan / VEG ER for Pets</span>
      <span class="entry-location">White Plains, NY</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Data Scientist Capstone Project Intern (Snowflake, Tableau)</span>
      <span class="entry-date"><time datetime="2025">Spring 2025</time> – Present</span>
    </div>
    <ul>
      <li>Developing an anomaly detection pipeline to flag veterinary cases across 128 emergency rooms for clinical audit review</li>
      <li>Extracting contextual embeddings from unstructured doctor notes using ClinicalBERT to represent clinical language for downstream anomaly detection</li>
      <li>Applying Isolation Forest models to unlabeled case data to identify outliers of unusual treatment or billing patterns</li>
    </ul>
  </div>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">MIT Sloan / Schneider Electric</span>
      <span class="entry-location">Cambridge, MA</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Analytics Lab Project Team Member (Python)</span>
      <span class="entry-date"><time datetime="2025">Fall 2025</time></span>
    </div>
    <ul>
      <li>Identified extreme weather events leveraging Mean–Seasonal–Trend Decomposition and Monte Carlo Sampling</li>
      <li>Estimated the causal impact of extreme weather on data-center cooling performance using Causal Forest models</li>
      <li>Developed an AI-driven early prediction system leveraging reinforcement learning to anticipate GPU overheating risks</li>
      <li>Projected $100k increase in incremental ARR by identifying how interpretable cooling-operations analytics could increase monitoring sales by 5–15% across a 55,000-device fleet</li>
    </ul>
  </div>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">Key Capture Energy</span>
      <span class="entry-location">Houston, TX / Brooklyn, NY</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Market Operations Analyst (Python, SQL)</span>
      <span class="entry-date"><time datetime="2022">Summer 2022</time>, <time datetime="2023">2023</time>–<time datetime="2024">2024</time></span>
    </div>
    <ul>
      <li>Automated battery trading, reducing manual intervention and enabling more reliable operations</li>
      <li>Constructed and iteratively improved short-term ML and fundamental price forecast models, increasing forecast accuracy and overall revenue</li>
      <li>Overhauled internal backtesting infrastructure and logic, cutting model evaluation time by 50%</li>
      <li>Identified rare market opportunities in day-ahead trading, leading all other batteries during the highest revenue month of 2024</li>
    </ul>
  </div>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">CELT Granted Professor Mentored Research</span>
      <span class="entry-location">New Orleans, LA</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Undergraduate Researcher (Stata, R)</span>
      <span class="entry-date"><time datetime="2022">2022</time>–<time datetime="2023">2023</time></span>
    </div>
    <ul>
      <li>Conducted research with two economics professors analyzing the rise of partisan sorting in U.S. colleges over 20 years</li>
      <li>Ran fixed-effects regression models to study how a college's political leaning correlates with student attendance patterns</li>
      <li>Presented findings at a university-wide poster session to faculty and 300+ students</li>
    </ul>
  </div>

  <div class="entry">
    <div class="entry-header">
      <span class="entry-title">Tulane Consulting Group</span>
      <span class="entry-location">New Orleans, LA</span>
    </div>
    <div class="entry-sub">
      <span class="entry-role">Founder, President</span>
      <span class="entry-date"><time datetime="2022">2022</time>–<time datetime="2023">2023</time></span>
    </div>
    <ul>
      <li>Crafted a leadership board of 6 students and grew club membership to 80+ students over two semesters</li>
      <li>Designed interviews, training systems, and organized resources used for recruiting twice a year</li>
    </ul>
  </div>
</section>

<section>
  <h2>Technical Skills</h2>
  <p>SQL, Python, R, Julia, Git, Gurobi, Tableau, Front-End Development, Visio, LaTeX, JuMP, PowerBI</p>
</section>

<section>
  <h2>Additional Information</h2>
  <p>Interests: Glassblowing, Skiing, Cooking, Backpacking</p>
</section>

<style>
  h1 {
    font-size: 2.2rem;
    margin-bottom: 1.5rem;
  }

  section {
    background: var(--surface, #fff);
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    padding: 1.5rem 2rem;
    margin-bottom: 1.5rem;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
  }

  h2 {
    font-size: 1.3rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--color-accent, #000000);
    border-bottom: 2px solid var(--color-accent, #ddd);
    padding-bottom: 0.4rem;
    margin-bottom: 1.2rem;
  }

  .entry {
    margin-bottom: 1.4rem;
  }

  .entry:last-child {
    margin-bottom: 0;
  }

  .entry-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
  }

  .entry-title {
    font-weight: 700;
    font-size: 1rem;
  }

  .entry-location {
    font-size: 0.85rem;
    color: #666;
  }

  .entry-sub {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-top: 0.15rem;
    margin-bottom: 0.5rem;
  }

  .entry-role {
    font-style: italic;
    font-size: 0.92rem;
  }

  .entry-date {
    font-size: 0.85rem;
    color: #666;
    white-space: nowrap;
  }

  ul {
    margin: 0.4rem 0 0 1.2rem;
    padding: 0;
  }

  li {
    font-size: 0.92rem;
    margin-bottom: 0.3rem;
    line-height: 1.5;
  }
</style>