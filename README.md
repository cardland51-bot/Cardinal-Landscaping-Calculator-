# Cardinal-Landscaping-Calculator-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cardinal Landscaping Price Calculator</title>
<style>
body {
    font-family: 'Arial', sans-serif;
    background: #fdfcfb;
    color: #333;
    padding: 0;
    margin: 0;
}
header {
    background: #800000;
    color: #fff;
    text-align: center;
    padding: 1rem 0;
    font-size: 1.8rem;
    font-weight: bold;
}
.container {
    max-width: 800px;
    margin: 2rem auto;
    padding: 2rem;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.1);
}
.step {
    display: none;
}
.step.active {
    display: block;
}
button {
    background: #800000;
    color: #fff;
    border: none;
    padding: 0.8rem 1.2rem;
    font-size: 1rem;
    border-radius: 8px;
    cursor: pointer;
    margin-top: 1rem;
}
button:hover {
    background: #a00000;
}
.custom-select {
    position: relative;
    display: inline-block;
    width: 100%;
    margin: 1rem 0;
}
.selected {
    padding: 0.8rem;
    border: 2px solid #800000;
    border-radius: 8px;
    cursor: pointer;
    background: #fff;
    font-size: 1rem;
    box-shadow: 0 0 6px rgba(128,0,0,0.3);
}
.options {
    position: absolute;
    width: 100%;
    top: 100%;
    left: 0;
    background: #fff;
    border: 2px solid #800000;
    border-top: none;
    max-height: 200px;
    overflow-y: auto;
    z-index: 100;
    display: none;
    border-radius: 0 0 8px 8px;
}
.option {
    padding: 0.8rem;
    cursor: pointer;
}
.option:hover {
    background: rgba(128,0,0,0.1);
}
#summary {
    margin-top: 2rem;
    font-size: 1.5rem;
    font-weight: bold;
    color: #800000;
}
.disclaimer {
    font-size: 0.8rem;
    color: #555;
    margin-top: 2rem;
    text-align: center;
}
input[type="checkbox"] {
    transform: scale(1.3);
    margin-right: 0.5rem;
    accent-color: #800000;
}
textarea {
    width: 100%;
    min-height: 80px;
    border: 2px solid #800000;
    border-radius: 8px;
    padding: 0.8rem;
    margin-top: 1rem;
}
.progress-bar {
    height: 10px;
    background: #eee;
    border-radius: 8px;
    overflow: hidden;
    margin: 1rem 0;
}
.progress {
    height: 100%;
    width: 0;
    background: #800000;
    transition: width 0.3s;
}
</style>
</head>
<body>
<header>Cardinal Landscaping Price Calculator</header>
<div class="container">
    <div class="progress-bar">
        <div class="progress" id="progress"></div>
    </div>

    <!-- Step 1: Customer Type -->
    <div class="step active">
        <div class="custom-select" id="customerSelect">
            <div class="selected" data-value="">First-time or Recurring?</div>
            <div class="options">
                <div class="option" data-value="first">First-time</div>
                <div class="option" data-value="recurring">Recurring</div>
            </div>
        </div>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 2: Package Size -->
    <div class="step">
        <div class="custom-select" id="packageSelect">
            <div class="selected" data-value="">Select Package</div>
            <div class="options">
                <div class="option" data-value="tiny">Tiny (Front bed only)</div>
                <div class="option" data-value="small">Small (Front + 1-2 island/corner beds)</div>
                <div class="option" data-value="medium">Medium (Front + sides + small back beds)</div>
                <div class="option" data-value="large">Large (Full front, back, sides + property line)</div>
            </div>
        </div>
        <button onclick="prevStep()">Back</button>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 3: Weed Severity -->
    <div class="step">
        <div class="custom-select" id="weedsSelect">
            <div class="selected" data-value="">Weed Level</div>
            <div class="options">
                <div class="option" data-value="light">Slight Weeds</div>
                <div class="option" data-value="moderate">Moderate Weeds</div>
                <div class="option" data-value="heavy">Heavy Weeds</div>
            </div>
        </div>
        <button onclick="prevStep()">Back</button>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 4: Shrub Overgrowth -->
    <div class="step">
        <div class="custom-select" id="overgrowthSelect">
            <div class="selected" data-value="">Shrub Overgrowth</div>
            <div class="options">
                <div class="option" data-value="light">Slightly Overgrown</div>
                <div class="option" data-value="moderate">Moderately Overgrown</div>
                <div class="option" data-value="heavy">Very Overgrown</div>
            </div>
        </div>
        <label><input type="checkbox" id="tallShrubs">Shrubs over 10 ft tall</label>
        <button onclick="prevStep()">Back</button>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 5: Attention to Detail -->
    <div class="step">
        <div class="custom-select" id="attentionSelect">
            <div class="selected" data-value="">Attention to Detail</div>
            <div class="options">
                <div class="option" data-value="normal">Normal</div>
                <div class="option" data-value="high">High / Premium</div>
            </div>
        </div>
        <button onclick="prevStep()">Back</button>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 6: Additional Info / Photos -->
    <div class="step">
        <textarea placeholder="Additional info / scope of work"></textarea>
        <input type="file" multiple>
        <button onclick="prevStep()">Back</button>
        <button onclick="nextStep()">Next</button>
    </div>

    <!-- Step 7: Summary -->
    <div class="step">
        <div id="summary">Estimated Total: $0</div>
        <div class="disclaimer">All pricing estimates are accurate within ±15%.</div>
        <button onclick="prevStep()">Back</button>
    </div>
</div>

<script>
let currentStep = 0;
const steps = document.querySelectorAll('.step');

function showStep(n){
    steps.forEach((step,index)=> step.classList.toggle('active', index===n));
    const progress = document.getElementById('progress');
    progress.style.width = ((n+1)/steps.length*100) + '%';
}

function nextStep(){
    if(currentStep < steps.length -1) currentStep++;
    showStep(currentStep);
    updateSummary();
}
function prevStep(){
    if(currentStep > 0) currentStep--;
    showStep(currentStep);
    updateSummary();
}

// Dropdown logic
document.querySelectorAll('.custom-select').forEach(select => {
    const selected = select.querySelector('.selected');
    const optionsContainer = select.querySelector('.options');
    const optionsList = select.querySelectorAll('.option');

    selected.addEventListener('click', ()=> {
        optionsContainer.style.display = optionsContainer.style.display === 'block' ? 'none' : 'block';
    });

    optionsList.forEach(option => {
        option.addEventListener('click', () => {
            selected.textContent = option.textContent;
            selected.dataset.value = option.dataset.value;
            optionsContainer.style.display = 'none';
            updateSummary();
        });
    });
});

document.addEventListener('click', (e) => {
    document.querySelectorAll('.custom-select').forEach(select => {
        if (!select.contains(e.target)) {
            select.querySelector('.options').style.display = 'none';
        }
    });
});

// Dynamic pricing
function updateSummary() {
    let total = 0;

    const packagePrices = { tiny: 250, small: 450, medium: 950, large: 1400 };
    const packageMax = { tiny: 550, small: 950, medium: 1850, large: 2900 };

    const packageSelect = document.getElementById('packageSelect').querySelector('.selected').dataset.value;
    const attentionSelect = document.getElementById('attentionSelect').querySelector('.selected').dataset.value;
    const weedsSelect = document.getElementById('weedsSelect').querySelector('.selected').dataset.value;
    const overgrowthSelect = document.getElementById('overgrowthSelect').querySelector('.selected').dataset.value;
    const tallShrubs = document.getElementById('tallShrubs').checked;

    if(packageSelect){
        total = packagePrices[packageSelect];

        // Attention to detail
        if(attentionSelect === 'normal') total += (packageMax[packageSelect]-total)*0.8;
        else if(attentionSelect === 'high') total = packageMax[packageSelect];

        // Weed severity
        if(weedsSelect === 'moderate') total *= 1.25;
        else if(weedsSelect === 'heavy') total *= 1.5;

        // Overgrowth
        if(overgrowthSelect === 'moderate') total *= 1.15;
        else if(overgrowthSelect === 'heavy') total *= 1.35;

        // Tall shrubs
        if(tallShrubs) total *= 1.25;

        total = Math.round(total);
    }

    document.getElementById('summary').textContent = "Estimated Total: $" + (total || "Select options above");
}

// Initialize
showStep(currentStep);
updateSummary();
</script>
</body>
</html>
