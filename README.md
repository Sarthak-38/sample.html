<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fake News Detector</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .spinner {
            border: 3px solid #f3f3f3;
            border-top: 3px solid #3498db;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .result-real {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
        }
        
        .result-fake {
            background: linear-gradient(135deg, #ef4444, #dc2626);
            color: white;
        }
        
        .result-uncertain {
            background: linear-gradient(135deg, #f59e0b, #d97706);
            color: white;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <!-- Header -->
    <header class="bg-white shadow-sm border-b">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-4 sm:py-6">
            <div class="flex items-center justify-center space-x-3">
                <div class="w-8 h-8 sm:w-10 sm:h-10 bg-blue-600 rounded-lg flex items-center justify-center">
                    <svg class="w-5 h-5 sm:w-6 sm:h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                    </svg>
                </div>
                <h1 class="text-2xl sm:text-3xl font-bold text-gray-900">Fake News Detector</h1>
            </div>
            <p class="text-center text-gray-600 mt-2 text-sm sm:text-base">Analyze news headlines and articles for authenticity</p>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8">
        <!-- Input Form -->
        <div class="bg-white rounded-xl shadow-lg p-6 sm:p-8 mb-6 sm:mb-8">
            <form id="newsForm" class="space-y-6">
                <div>
                    <label for="newsInput" class="block text-sm font-medium text-gray-700 mb-2">
                        Enter news headline or article text
                    </label>
                    <textarea 
                        id="newsInput" 
                        rows="4" 
                        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none transition-all duration-200 text-sm sm:text-base"
                        placeholder="Paste your news headline or article text here..."
                        required
                    ></textarea>
                </div>
                
                <!-- Removed API key input field -->
                
                <button 
                    type="submit" 
                    id="submitBtn"
                    class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-lg transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 text-sm sm:text-base"
                >
                    Analyze News
                </button>
            </form>
        </div>

        <!-- Loading Spinner -->
        <div id="loadingSpinner" class="hidden bg-white rounded-xl shadow-lg p-6 sm:p-8 text-center">
            <div class="spinner"></div>
            <p class="text-gray-600 mt-4 text-sm sm:text-base">Analyzing content...</p>
        </div>

        <!-- Results Area -->
        <div id="resultArea" class="hidden">
            <div id="resultCard" class="rounded-xl shadow-lg p-6 sm:p-8 text-center fade-in">
                <div id="resultIcon" class="w-12 h-12 sm:w-16 sm:h-16 mx-auto mb-4 rounded-full flex items-center justify-center">
                    <!-- Icon will be inserted here -->
                </div>
                <h2 id="resultTitle" class="text-xl sm:text-2xl font-bold mb-2"></h2>
                <p id="resultDescription" class="text-base sm:text-lg opacity-90 mb-4"></p>
                <div id="confidenceScore" class="text-xs sm:text-sm opacity-75"></div>
                <!-- Added detailed analysis section -->
                <div id="detailedAnalysis" class="mt-6 p-4 bg-black bg-opacity-10 rounded-lg text-left">
                    <h3 class="font-semibold mb-2 text-sm sm:text-base">Analysis Details:</h3>
                    <ul id="analysisList" class="text-xs sm:text-sm space-y-1"></ul>
                </div>
            </div>
        </div>

        <!-- Error Area -->
        <div id="errorArea" class="hidden">
            <div class="bg-red-50 border border-red-200 rounded-xl p-6 text-center">
                <div class="w-12 h-12 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-4">
                    <svg class="w-6 h-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                    </svg>
                </div>
                <h3 class="text-lg font-semibold text-red-800 mb-2">Analysis Failed</h3>
                <p id="errorMessage" class="text-red-600 text-sm sm:text-base"></p>
                <button 
                    id="retryBtn"
                    class="mt-4 bg-red-600 hover:bg-red-700 text-white font-medium py-2 px-4 rounded-lg transition-colors duration-200 text-sm sm:text-base"
                >
                    Try Again
                </button>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-white border-t mt-12 sm:mt-16">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-4 sm:py-6 text-center text-gray-600">
            <p class="text-xs sm:text-sm">&copy; 2024 Fake News Detector. Built with HTML, CSS, and JavaScript.</p>
        </div>
    </footer>

    <script>
        class FakeNewsDetector {
            constructor() {
                this.form = document.getElementById('newsForm');
                this.newsInput = document.getElementById('newsInput');
                this.submitBtn = document.getElementById('submitBtn');
                this.loadingSpinner = document.getElementById('loadingSpinner');
                this.resultArea = document.getElementById('resultArea');
                this.errorArea = document.getElementById('errorArea');
                this.retryBtn = document.getElementById('retryBtn');
                
                this.initEventListeners();
            }
            
            initEventListeners() {
                this.form.addEventListener('submit', (e) => this.handleSubmit(e));
                this.retryBtn.addEventListener('click', () => this.hideError());
            }
            
            async handleSubmit(e) {
                e.preventDefault();
                
                const newsText = this.newsInput.value.trim();
                
                if (!newsText) {
                    this.showError('Please enter some news text to analyze.');
                    return;
                }
                
                this.showLoading();
                
                try {
                    const result = await this.analyzeNews(newsText);
                    this.showResult(result);
                } catch (error) {
                    this.showError(error.message);
                }
            }
            
            async analyzeNews(text) {
                return new Promise((resolve) => {
                    setTimeout(() => {
                        const analysis = this.performAdvancedAnalysis(text);
                        resolve(analysis);
                    }, 2000);
                });
            }
            
            performAdvancedAnalysis(text) {
                const textLower = text.toLowerCase();
                const words = textLower.split(/\s+/);
                const wordCount = words.length;
                
                // Enhanced detection patterns
                const fakeIndicators = {
                    sensational: ['shocking', 'unbelievable', 'amazing', 'incredible', 'mind-blowing', 'you won\'t believe'],
                    clickbait: ['doctors hate', 'this one trick', 'secret', 'they don\'t want you to know', 'exposed'],
                    emotional: ['outrageous', 'disgusting', 'terrifying', 'devastating', 'horrific'],
                    urgency: ['breaking', 'urgent', 'immediate', 'act now', 'before it\'s too late'],
                    conspiracy: ['cover-up', 'hidden truth', 'conspiracy', 'they\'re hiding', 'wake up']
                };
                
                const realIndicators = {
                    credible: ['according to', 'study shows', 'research indicates', 'data suggests', 'experts say'],
                    sources: ['university', 'institute', 'published', 'peer-reviewed', 'journal'],
                    neutral: ['reported', 'stated', 'announced', 'confirmed', 'official'],
                    factual: ['statistics', 'evidence', 'findings', 'analysis', 'investigation']
                };
                
                let fakeScore = 0;
                let realScore = 0;
                let detailedFindings = [];
                
                // Check for fake indicators
                Object.entries(fakeIndicators).forEach(([category, keywords]) => {
                    const matches = keywords.filter(keyword => textLower.includes(keyword));
                    if (matches.length > 0) {
                        fakeScore += matches.length * 2;
                        detailedFindings.push(`Contains ${category} language: "${matches.join('", "')}"`);
                    }
                });
                
                // Check for real indicators
                Object.entries(realIndicators).forEach(([category, keywords]) => {
                    const matches = keywords.filter(keyword => textLower.includes(keyword));
                    if (matches.length > 0) {
                        realScore += matches.length * 2;
                        detailedFindings.push(`Contains ${category} language: "${matches.join('", "')}"`);
                    }
                });
                
                // Additional checks
                const hasNumbers = /\d/.test(text);
                const hasQuotes = text.includes('"') || text.includes("'");
                const hasCapitalization = /[A-Z]{3,}/.test(text);
                const hasExclamation = (text.match(/!/g) || []).length;
                
                if (hasNumbers) {
                    realScore += 1;
                    detailedFindings.push('Contains numerical data');
                }
                
                if (hasQuotes) {
                    realScore += 1;
                    detailedFindings.push('Contains quoted statements');
                }
                
                if (hasCapitalization) {
                    fakeScore += 2;
                    detailedFindings.push('Contains excessive capitalization');
                }
                
                if (hasExclamation > 2) {
                    fakeScore += hasExclamation;
                    detailedFindings.push(`Contains excessive exclamation marks (${hasExclamation})`);
                }
                
                // Length analysis
                if (wordCount < 10) {
                    detailedFindings.push('Very short text - limited analysis possible');
                } else if (wordCount > 100) {
                    realScore += 1;
                    detailedFindings.push('Substantial content length');
                }
                
                // Determine classification
                let classification, confidence;
                const scoreDifference = Math.abs(fakeScore - realScore);
                
                if (fakeScore > realScore + 2) {
                    classification = 'fake';
                    confidence = Math.min(60 + (scoreDifference * 5), 95);
                } else if (realScore > fakeScore + 2) {
                    classification = 'real';
                    confidence = Math.min(60 + (scoreDifference * 5), 95);
                } else {
                    classification = 'uncertain';
                    confidence = 50 + Math.random() * 20; // 50-70% for uncertain
                }
                
                return {
                    classification,
                    confidence: Math.round(confidence),
                    analysis: this.getAnalysisText(classification),
                    detailedFindings: detailedFindings.length > 0 ? detailedFindings : ['No specific indicators found - analysis based on general patterns'],
                    scores: { fake: fakeScore, real: realScore }
                };
            }
            
            getAnalysisText(classification) {
                const analyses = {
                    real: 'This content appears to be legitimate news based on language patterns and credibility indicators.',
                    fake: 'This content shows characteristics commonly associated with misinformation or fake news.',
                    uncertain: 'The analysis is inconclusive. Consider verifying with additional sources.'
                };
                return analyses[classification];
            }
            
            showLoading() {
                this.hideAllSections();
                this.loadingSpinner.classList.remove('hidden');
                this.submitBtn.disabled = true;
                this.submitBtn.textContent = 'Analyzing...';
            }
            
            showResult(result) {
                this.hideAllSections();
                
                const resultCard = document.getElementById('resultCard');
                const resultIcon = document.getElementById('resultIcon');
                const resultTitle = document.getElementById('resultTitle');
                const resultDescription = document.getElementById('resultDescription');
                const confidenceScore = document.getElementById('confidenceScore');
                const analysisList = document.getElementById('analysisList');
                
                // Set result styling and content based on classification
                if (result.classification === 'real') {
                    resultCard.className = 'rounded-xl shadow-lg p-6 sm:p-8 text-center fade-in result-real';
                    resultIcon.innerHTML = `
                        <svg class="w-6 h-6 sm:w-8 sm:h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                    `;
                    resultTitle.textContent = 'Likely Real News';
                } else if (result.classification === 'fake') {
                    resultCard.className = 'rounded-xl shadow-lg p-6 sm:p-8 text-center fade-in result-fake';
                    resultIcon.innerHTML = `
                        <svg class="w-6 h-6 sm:w-8 sm:h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    `;
                    resultTitle.textContent = 'Likely Fake News';
                } else {
                    resultCard.className = 'rounded-xl shadow-lg p-6 sm:p-8 text-center fade-in result-uncertain';
                    resultIcon.innerHTML = `
                        <svg class="w-6 h-6 sm:w-8 sm:h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                    `;
                    resultTitle.textContent = 'Uncertain';
                }
                
                resultDescription.textContent = result.analysis;
                confidenceScore.textContent = `Confidence: ${result.confidence}%`;
                
                analysisList.innerHTML = '';
                result.detailedFindings.forEach(finding => {
                    const li = document.createElement('li');
                    li.textContent = `• ${finding}`;
                    analysisList.appendChild(li);
                });
                
                this.resultArea.classList.remove('hidden');
                this.resetForm();
            }
            
            showError(message) {
                this.hideAllSections();
                document.getElementById('errorMessage').textContent = message;
                this.errorArea.classList.remove('hidden');
                this.resetForm();
            }
            
            hideError() {
                this.errorArea.classList.add('hidden');
            }
            
            hideAllSections() {
                this.loadingSpinner.classList.add('hidden');
                this.resultArea.classList.add('hidden');
                this.errorArea.classList.add('hidden');
            }
            
            resetForm() {
                this.submitBtn.disabled = false;
                this.submitBtn.textContent = 'Analyze News';
            }
        }
        
        // Initialize the application
        document.addEventListener('DOMContentLoaded', () => {
            new FakeNewsDetector();
        });
    </script>
</body>
</html>
