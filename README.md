# Getting Started with GitHub Copilot

<img src="https://octodex.github.com/images/Professortocat_v2.png" align="right" height="200px" />

Hey @BenjaBaks!

Mona here. I'm done preparing your exercise. Hope you enjoy! 💚

Remember, it's self-paced so feel free to take a break! ☕️

[![](https://img.shields.io/badge/Go%20to%20Exercise-%E2%86%92-1f883d?style=for-the-badge&logo=github&labelColor=197935)](https://github.com/BenjaBaks/skills-getting-started-with-github-copilot/issues/1)

---

&copy; 2025 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>African Women's Ancestry Cancer PRS Dashboard | IDI-ACE/SHEDS</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <link href="https://cdn.jsdelivr.net/npm/daisyui@3.9.4/dist/full.css" rel="stylesheet">
  <style>
    .plot-container { height: 400px; width: 100%; }
    .upload-area {
      transition: all 0.3s ease;
      min-height: 150px;
    }
    .upload-area.drag-over {
      background-color: #f0f9ff;
      border-color: #2E5C87 !important;
    }
    @media (max-width: 768px) {
      .dashboard-grid { grid-template-columns: 1fr !important; }
      .filter-tabs { overflow-x: auto; white-space: nowrap; }
    }
  </style>
</head>
<body class="bg-gray-50">
  <!-- Header -->
  <header class="bg-[#2E5C87] text-white p-4">
    <div class="flex flex-col md:flex-row md:justify-between md:items-center">
      <div>
        <h1 class="text-2xl font-bold">African Women's Ancestry Cancer PRS Dashboard</h1>
        <p class="text-sm italic">An oncology tool for guiding clinical practice and research</p>
        <p class="text-xs mt-1">Validated Model: LASSO (AUC 0.98) | XGBoost (AUC 0.96)</p>
      </div>
      <select class="select select-bordered select-sm w-full md:w-64 mt-2 md:mt-0" id="datasetSelector">
        <option disabled selected>Select Dataset</option>
        <option>UK Biobank (European)</option>
        <option>Duke/UCI-Fred Hutch</option>
        <option selected>H3Africa</option>
        <option>IDI-ACE</option>
        <option selected>Uganda Cancer Institute</option>
      </select>
    </div>
  </header>

  <!-- Dashboard Grid -->
  <div class="dashboard-grid grid grid-cols-1 md:grid-cols-12 gap-4 p-4">
    <!-- Left Panel -->
    <div class="md:col-span-3 space-y-4">
      <!-- Functional Drag-and-Drop Upload -->
      <div class="bg-white p-4 rounded-lg shadow">
        <h2 class="font-bold text-[#2E5C87] mb-2">Data Upload</h2>
        <div id="uploadArea" class="upload-area border-2 border-dashed border-gray-300 rounded-lg p-8 text-center cursor-pointer">
          <p class="text-sm">Drag & drop VCF/CSV files here</p>
          <p class="text-xs text-gray-500 mt-1">Supports: UKB, H3Africa formats</p>
          <input type="file" id="fileInput" class="hidden" accept=".vcf,.csv,.tsv">
          <div id="fileInfo" class="mt-2 text-sm text-[#2E5C87] hidden"></div>
        </div>
      </div>

      <!-- Fully Interactive Filter Tabs -->
      <div class="bg-white p-4 rounded-lg shadow">
        <h2 class="font-bold text-[#2E5C87] mb-2">Filter Variables (100+)</h2>
        <div class="filter-tabs tabs tabs-boxed">
          <a class="tab tab-active" onclick="showFilterTab('clinical')">Clinical</a> 
          <a class="tab" onclick="showFilterTab('genomic')">Genomic</a> 
          <a class="tab" onclick="showFilterTab('environmental')">Environmental</a>
        </div>
        
        <!-- Clinical Filters (Default Visible) -->
        <div id="clinicalFilters" class="filter-content mt-2 max-h-96 overflow-y-auto">
          <div class="space-y-2">
            <div class="filter-group">
              <div class="filter-group-title">Demographics</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="demographics" />
                <span class="text-sm">Age (increasing years)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="demographics" />
                <span class="text-sm">Sex (female)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="demographics" />
                <span class="text-sm">Race/ethnicity</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="demographics" />
                <span class="text-sm">Socioeconomic status / education level</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="demographics" />
                <span class="text-sm">Health-care access / screening adherence</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Reproductive History</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" checked class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Early menarche (< 12 years)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Late menopause (> 55 years)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Nulliparity (no full-term pregnancies)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Late first full-term pregnancy (> 30 years)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Never breast-fed</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Parity number (total pregnancies)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Polycystic ovary syndrome (PCOS)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Endometriosis</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="reproductive" />
                <span class="text-sm">Infertility and fertility-drug exposure</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Medical History</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Personal history of benign breast disease</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Personal history of prior breast/gynecologic malignancy</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Hormone-replacement therapy use (estrogen+progesterone)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Long-term oral contraceptive use</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Type 2 diabetes mellitus</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Metabolic syndrome</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Hypertension</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Chronic inflammation (e.g. mastitis)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Autoimmune disease (e.g. lupus)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Immunosuppression (e.g. HIV infection, transplant)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Tumor grade (for existing cancer)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" checked class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Tumor stage at diagnosis</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Hormone-receptor status (ER/PR/HER2)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="medical" />
                <span class="text-sm">Histological subtype (ductal vs. lobular, etc.)</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Lifestyle Factors</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Alcohol consumption history</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Tobacco smoking status</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Physical inactivity</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Diet: high red/processed meat intake</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Diet: low fiber intake</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Vitamin D deficiency</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Stress and psychosocial factors</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="lifestyle" />
                <span class="text-sm">Sleep quality / circadian disruption</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Physical Characteristics</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">High breast density</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">Body mass index (BMI) / obesity</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">Waist-hip ratio (central obesity)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">Elevated circulating estrogen levels</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">Skin phototype (Fitzpatrick I–VI)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs clinical-filter" data-category="physical" />
                <span class="text-sm">Number of benign nevi (for melanoma risk)</span>
              </label>
            </div>
          </div>
        </div>
        
        <!-- Genomic Filters (Visible) -->
        <div id="genomicFilters" class="filter-content visible mt-2 max-h-96 overflow-y-auto">
          <div class="space-y-2">
            <div class="filter-group">
              <div class="filter-group-title">Hereditary Factors</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="hereditary" />
                <span class="text-sm">Family history of breast/gynecologic cancer</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Germline Mutations</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" checked class="checkbox checkbox-xs genomic-filter" data-category="mutations" />
                <span class="text-sm">Germline mutations: BRCA1</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="mutations" />
                <span class="text-sm">Germline mutations: BRCA2</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="mutations" />
                <span class="text-sm">Germline mutations: TP53 (Li–Fraumeni)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="mutations" />
                <span class="text-sm">Germline mutations: PTEN (Cowden)</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">DNA Repair Mechanisms</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="dna-repair" />
                <span class="text-sm">DNA mismatch-repair defects (Lynch syndrome)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs genomic-filter" data-category="dna-repair" />
                <span class="text-sm">DNA-repair capacity polymorphisms</span>
              </label>
            </div>
          </div>
        </div>
        
        <!-- Environmental Filters (Visible) -->
        <div id="environmentalFilters" class="filter-content visible mt-2 max-h-96 overflow-y-auto">
          <div class="space-y-2">
            <div class="filter-group">
              <div class="filter-group-title">Infectious Agents</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" checked class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">High-risk HPV infection (16/18)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">Chlamydia trachomatis infection</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">Epstein–Barr virus (EBV)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">Herpes simplex virus (HSV-2)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">Hepatitis B/C virus</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="infectious" />
                <span class="text-sm">Human immunodeficiency virus (HIV) co-infection</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Chemical Exposures</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Persistent organic pollutants (POPs)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Endocrine disruptors: BPA</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Endocrine disruptors: phthalates</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Polychlorinated biphenyls (PCBs)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Dioxins (TCDD)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Pesticides: DDT, organochlorines</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Volatile organic compounds (VOCs)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Microplastics / nanoplastics</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Food-processing by-products: nitrosamines</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Mycotoxins (aflatoxin)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Nitrate/nitrite in processed meats</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Industrial solvents (trichloroethylene)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Hormone-disrupting agricultural chemicals</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Persistent endocrine-active pollutants (PFAS)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Nail-salon and hair-dye chemicals (aromatic amines)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Rubber-manufacturing exposures (nitrosamines)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Industrial dyes (azo dyes)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="chemical" />
                <span class="text-sm">Chlorinated drinking-water by-products (tri-halomethanes)</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Air & Environmental Pollutants</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Air pollution (PM2.5, PAHs)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Diesel exhaust</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Indoor biomass smoke (cooking fuels)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Asbestos fiber exposure</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Silica dust</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Welding fumes</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="pollutants" />
                <span class="text-sm">Volcanic ash (silica plus metal toxins)</span>
              </label>
            </div>
            
            <div class="filter-group">
              <div class="filter-group-title">Radiation Exposure</div>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Radon gas exposure</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Medical ionizing radiation (X-rays, CT scans)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Ultraviolet radiation (sunlight / tanning beds)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Radium (legacy contamination)</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">High-altitude UV index</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Indoor radon from building materials</span>
              </label>
              <label class="flex items-center gap-2">
                <input type="checkbox" class="checkbox checkbox-xs environmental-filter" data-category="radiation" />
                <span class="text-sm">Phosphate-fertilizer dust (radioactive residues)</span>
              </label>
            </div>
        </div>
        </div>

        <button id="applyFilters" class="btn btn-sm btn-primary mt-4 w-full">Apply Filters (3 Selected)</button>
      </div>

      <!-- Model Selection -->
      <div class="bg-white p-4 rounded-lg shadow">
        <h2 class="font-bold text-[#2E5C87] mb-2">PRS Model</h2>
        <select class="select select-bordered select-sm w-full">
          <option>LD Clumping + Thresholding</option>
          <option selected>LASSO Regression</option>
          <option>XGBoost</option>
        </select>
        <div class="stats shadow mt-2">
          <div class="stat">
            <div class="stat-title">AUC</div>
            <div class="stat-value">0.98</div>
            <div class="stat-desc">5-fold CV</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Right Panel -->
    <div class="md:col-span-9">
      <!-- Visualization Tabs -->
      <div class="tabs tabs-boxed">
        <a class="tab tab-active" onclick="showVizTab('distribution')">Distribution</a> 
        <a class="tab" onclick="showVizTab('scatter')">Scatter Plot</a> 
        <a class="tab" onclick="showVizTab('roc')">ROC Curve</a>
        <a class="tab" onclick="showVizTab('risk')">High-Risk Table</a>
      </div>

      <!-- Tab Contents -->
      <div class="bg-white p-4 rounded-b-lg shadow">
        <!-- Distribution Plot -->
        <div id="distributionViz" class="viz-content">
          <h3 class="font-bold text-[#2E5C87] mb-4">PRS Distribution by Dataset</h3>
          <div id="distPlot" class="plot-container"></div>
          <div class="flex justify-end mt-4">
            <button class="btn btn-sm">Download CSV</button>
          </div>
        </div>

        <!-- Other Visualization Tabs (Same as before) -->
        <!-- ... -->
      </div>
    </div>
  </div>

  <script>
    // Initialize all plots
    function initPlots() {
      // ... (Same plot initialization as before)
    }

    // DRAG-AND-DROP FUNCTIONALITY
    const uploadArea = document.getElementById('uploadArea');
    const fileInput = document.getElementById('fileInput');
    const fileInfo = document.getElementById('fileInfo');

    uploadArea.addEventListener('click', () => fileInput.click());
    
    // Handle drag over
    uploadArea.addEventListener('dragover', (e) => {
      e.preventDefault();
      uploadArea.classList.add('drag-over');
    });

    // Handle drag leave
    uploadArea.addEventListener('dragleave', () => {
      uploadArea.classList.remove('drag-over');
    });

    // Handle drop
    uploadArea.addEventListener('drop', (e) => {
      e.preventDefault();
      uploadArea.classList.remove('drag-over');
      
      if (e.dataTransfer.files.length) {
        handleFiles(e.dataTransfer.files);
      }
    });

    // Handle file selection
    fileInput.addEventListener('change', () => {
      if (fileInput.files.length) {
        handleFiles(fileInput.files);
      }
    });

    function handleFiles(files) {
      const file = files[0];
      fileInfo.textContent = `Ready: ${file.name} (${(file.size/1024/1024).toFixed(2)} MB)`;
      fileInfo.classList.remove('hidden');
      
      // Simulate file processing (replace with actual API call)
      setTimeout(() => {
        fileInfo.innerHTML += `<br><span class="text-green-600">✓ Processed successfully</span>`;
      }, 1500);
    }

    // FILTER TAB FUNCTIONALITY
    function showFilterTab(tabName) {
      // Hide all filter tabs
      document.querySelectorAll('.filter-content').forEach(tab => {
        tab.classList.add('hidden');
      });
      
      // Show selected tab
      document.getElementById(`${tabName}Filters`).classList.remove('hidden');
      
      // Update active tab styling
      document.querySelectorAll('.filter-tabs a').forEach(tab => {
        tab.classList.remove('tab-active');
      });
      event.target.classList.add('tab-active');
      
      // Update selected count
      updateSelectedCount();
    }

    // Update selected filter count
    function updateSelectedCount() {
      const selected = document.querySelectorAll('input[type="checkbox"]:checked').length;
      document.getElementById('applyFilters').textContent = `Apply Filters (${selected} Selected)`;
    }

    // Attach event listeners to all checkboxes
    document.querySelectorAll('input[type="checkbox"]').forEach(checkbox => {
      checkbox.addEventListener('change', updateSelectedCount);
    });

    // Initialize on load
    document.addEventListener('DOMContentLoaded', function() {
      initPlots();
      updateSelectedCount();
    });
  </script>
</body>
</html>
