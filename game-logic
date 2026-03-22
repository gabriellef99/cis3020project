// Paycheck to Paycheck - Game Logic
// Browser-based version with localStorage

// Game state
let gameState = {
    level: '',
    currentMonth: 1,
    balance: 0,
    creditScore: 650,
    stressLevel: 50,
    monthlyIncome: 0,
    currentDecisionIndex: 0,
    monthlyDecisions: [],
    totalDecisions: [],
    isGameOver: false
};

// Level configurations
const LEVEL_CONFIG = {
    starter: {
        name: 'Fresh Graduate',
        startingBalance: 3000,
        monthlyIncome: 3500,
        totalMonths: 12
    },
    intermediate: {
        name: 'Gig Economy Worker',
        startingBalance: 2500,
        monthlyIncome: 3000, // Variable in actual gameplay
        totalMonths: 12
    },
    expert: {
        name: 'Family Provider',
        startingBalance: 2000,
        monthlyIncome: 4200,
        totalMonths: 12
    }
};

// Decision scenarios by level
const SCENARIOS = {
    starter: [
        {
            title: 'Rent Payment',
            description: 'Your monthly rent is due. Pay now to maintain good credit.',
            amount: 1200,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -10, credit: 3, balance: -1200 },
                delay: { stress: 15, credit: -5, balance: 0 },
                skip: { stress: 25, credit: -15, balance: 0 }
            }
        },
        {
            title: 'Groceries',
            description: 'Weekly grocery shopping. Healthy eating is important.',
            amount: 150,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -5, credit: 0, balance: -150 },
                delay: { stress: 10, credit: 0, balance: 0 },
                skip: { stress: 20, credit: -2, balance: 0 }
            }
        },
        {
            title: 'Car Insurance',
            description: 'Monthly auto insurance premium is due.',
            amount: 120,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -5, credit: 2, balance: -120 },
                delay: { stress: 12, credit: -8, balance: 0 },
                skip: { stress: 30, credit: -20, balance: 0 }
            }
        },
        {
            title: 'Streaming Subscriptions',
            description: 'Netflix, Spotify, and other monthly subscriptions.',
            amount: 45,
            type: 'expense',
            category: 'Discretionary',
            impact: {
                pay: { stress: -2, credit: 0, balance: -45 },
                delay: { stress: 5, credit: 0, balance: 0 },
                skip: { stress: 3, credit: 0, balance: 45 }
            }
        },
        {
            title: 'Gym Membership',
            description: 'Monthly gym membership fee.',
            amount: 50,
            type: 'expense',
            category: 'Discretionary',
            impact: {
                pay: { stress: -3, credit: 0, balance: -50 },
                delay: { stress: 4, credit: 0, balance: 0 },
                skip: { stress: 8, credit: 0, balance: 50 }
            }
        },
        {
            title: 'Friend\'s Birthday Dinner',
            description: 'Your friend invited you to an expensive restaurant for their birthday.',
            amount: 80,
            type: 'expense',
            category: 'Social',
            impact: {
                pay: { stress: -5, credit: 0, balance: -80 },
                delay: { stress: 10, credit: 0, balance: 0 },
                skip: { stress: 15, credit: 0, balance: 80 }
            }
        },
        {
            title: 'Car Repair',
            description: 'Check engine light is on. Mechanic says it needs fixing.',
            amount: 450,
            type: 'expense',
            category: 'Emergency',
            impact: {
                pay: { stress: -8, credit: 0, balance: -450 },
                delay: { stress: 20, credit: 0, balance: 0 },
                skip: { stress: 35, credit: -5, balance: 0 }
            }
        },
        {
            title: 'Freelance Project Bonus',
            description: 'A client is offering extra payment for quick turnaround work.',
            amount: 300,
            type: 'income',
            category: 'Opportunity',
            impact: {
                pay: { stress: 10, credit: 0, balance: 300 },
                delay: { stress: 5, credit: 0, balance: 150 },
                skip: { stress: -5, credit: 0, balance: 0 }
            }
        }
    ],
    intermediate: [
        {
            title: 'Rent Payment',
            description: 'Monthly rent in an expensive city.',
            amount: 1800,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -10, credit: 3, balance: -1800 },
                delay: { stress: 20, credit: -10, balance: 0 },
                skip: { stress: 40, credit: -25, balance: 0 }
            }
        },
        {
            title: 'Student Loan Payment',
            description: 'Monthly student loan installment is due.',
            amount: 350,
            type: 'expense',
            category: 'Debt',
            impact: {
                pay: { stress: -5, credit: 5, balance: -350 },
                delay: { stress: 15, credit: -15, balance: 0 },
                skip: { stress: 30, credit: -30, balance: 0 }
            }
        },
        {
            title: 'Health Insurance',
            description: 'Self-employed health insurance premium.',
            amount: 280,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -8, credit: 0, balance: -280 },
                delay: { stress: 25, credit: 0, balance: 0 },
                skip: { stress: 40, credit: -5, balance: 0 }
            }
        },
        {
            title: 'Laptop Repair',
            description: 'Your work laptop crashed. Need it for gig work.',
            amount: 500,
            type: 'expense',
            category: 'Emergency',
            impact: {
                pay: { stress: -10, credit: 0, balance: -500 },
                delay: { stress: 30, credit: 0, balance: 0 },
                skip: { stress: 50, credit: -10, balance: 0 }
            }
        },
        {
            title: 'Networking Event',
            description: 'Industry networking event that could lead to more work.',
            amount: 120,
            type: 'expense',
            category: 'Investment',
            impact: {
                pay: { stress: -5, credit: 0, balance: -120 },
                delay: { stress: 8, credit: 0, balance: 0 },
                skip: { stress: 12, credit: 0, balance: 120 }
            }
        },
        {
            title: 'Emergency Gig Opportunity',
            description: 'Last-minute project pays well but requires weekend work.',
            amount: 600,
            type: 'income',
            category: 'Opportunity',
            impact: {
                pay: { stress: 15, credit: 0, balance: 600 },
                delay: { stress: 10, credit: 0, balance: 300 },
                skip: { stress: -5, credit: 0, balance: 0 }
            }
        }
    ],
    expert: [
        {
            title: 'Mortgage Payment',
            description: 'Monthly mortgage payment for your family home.',
            amount: 2200,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -10, credit: 5, balance: -2200 },
                delay: { stress: 30, credit: -20, balance: 0 },
                skip: { stress: 50, credit: -40, balance: 0 }
            }
        },
        {
            title: 'Childcare',
            description: 'Monthly daycare fees for two children.',
            amount: 1400,
            type: 'expense',
            category: 'Essential',
            impact: {
                pay: { stress: -12, credit: 0, balance: -1400 },
                delay: { stress: 35, credit: -10, balance: 0 },
                skip: { stress: 60, credit: -15, balance: 0 }
            }
        },
        {
            title: 'Medical Bills',
            description: 'Unexpected medical expense for your child.',
            amount: 800,
            type: 'expense',
            category: 'Emergency',
            impact: {
                pay: { stress: -15, credit: 0, balance: -800 },
                delay: { stress: 40, credit: -5, balance: 0 },
                skip: { stress: 70, credit: -20, balance: 0 }
            }
        },
        {
            title: 'Family Car Broke Down',
            description: 'Transmission failure. Need car for work and kids.',
            amount: 1200,
            type: 'expense',
            category: 'Emergency',
            impact: {
                pay: { stress: -20, credit: 0, balance: -1200 },
                delay: { stress: 50, credit: 0, balance: 0 },
                skip: { stress: 80, credit: -15, balance: 0 }
            }
        },
        {
            title: 'Home Repair',
            description: 'Water heater needs replacing immediately.',
            amount: 900,
            type: 'expense',
            category: 'Emergency',
            impact: {
                pay: { stress: -18, credit: 0, balance: -900 },
                delay: { stress: 45, credit: 0, balance: 0 },
                skip: { stress: 75, credit: -10, balance: 0 }
            }
        },
        {
            title: 'Overtime Opportunity',
            description: 'Extra shifts available this month.',
            amount: 800,
            type: 'income',
            category: 'Opportunity',
            impact: {
                pay: { stress: 20, credit: 0, balance: 800 },
                delay: { stress: 10, credit: 0, balance: 400 },
                skip: { stress: -8, credit: 0, balance: 0 }
            }
        }
    ]
};

// Navigation functions
function showLanding() {
    document.getElementById('landing-page').style.display = 'block';
    document.getElementById('level-select').style.display = 'none';
    document.getElementById('game-screen').style.display = 'none';
    document.getElementById('game-over').style.display = 'none';
}

function showLevelSelect() {
    document.getElementById('landing-page').style.display = 'none';
    document.getElementById('level-select').style.display = 'block';
    document.getElementById('game-screen').style.display = 'none';
    document.getElementById('game-over').style.display = 'none';
}

function showGameScreen() {
    document.getElementById('landing-page').style.display = 'none';
    document.getElementById('level-select').style.display = 'none';
    document.getElementById('game-screen').style.display = 'block';
    document.getElementById('game-over').style.display = 'none';
}

function showGameOver() {
    document.getElementById('landing-page').style.display = 'none';
    document.getElementById('level-select').style.display = 'none';
    document.getElementById('game-screen').style.display = 'none';
    document.getElementById('game-over').style.display = 'block';
}

function showMenu() {
    const confirmQuit = confirm('Are you sure you want to quit this game? Your progress will be lost.');
    if (confirmQuit) {
        showLevelSelect();
    }
}

// Game initialization
function startGame(level) {
    const config = LEVEL_CONFIG[level];
    
    gameState = {
        level: level,
        levelName: config.name,
        currentMonth: 1,
        totalMonths: config.totalMonths,
        balance: config.startingBalance,
        creditScore: 650,
        stressLevel: 50,
        monthlyIncome: config.monthlyIncome,
        currentDecisionIndex: 0,
        monthlyDecisions: [],
        totalDecisions: [],
        isGameOver: false
    };
    
    // Generate decisions for the month
    generateMonthlyDecisions();
    
    // Show game screen
    showGameScreen();
    
    // Update UI
    updateGameUI();
    
    // Show first decision
    showNextDecision();
}

// Generate random decisions for the month
function generateMonthlyDecisions() {
    const scenarios = SCENARIOS[gameState.level];
    const numberOfDecisions = 5 + Math.floor(Math.random() * 3); // 5-7 decisions per month
    
    gameState.monthlyDecisions = [];
    
    for (let i = 0; i < numberOfDecisions; i++) {
        const randomScenario = scenarios[Math.floor(Math.random() * scenarios.length)];
        gameState.monthlyDecisions.push({...randomScenario});
    }
    
    gameState.currentDecisionIndex = 0;
}

// Show next decision
function showNextDecision() {
    if (gameState.currentDecisionIndex >= gameState.monthlyDecisions.length) {
        // Month is complete, enable next month button
        document.querySelector('.next-month-button').disabled = false;
        document.getElementById('card-title').textContent = 'Month Complete!';
        document.getElementById('card-description').textContent = 'Review your decisions and click "Complete Month" to continue.';
        document.querySelector('.card-actions').style.display = 'none';
        document.querySelector('.card-category').style.display = 'none';
        document.querySelector('.card-amount').style.display = 'none';
        return;
    }
    
    const decision = gameState.monthlyDecisions[gameState.currentDecisionIndex];
    
    // Update card UI
    document.getElementById('card-category').textContent = decision.category;
    document.getElementById('card-title').textContent = decision.title;
    document.getElementById('card-description').textContent = decision.description;
    
    const amountElement = document.getElementById('card-amount');
    if (decision.type === 'income') {
        amountElement.textContent = `+$${decision.amount}`;
        amountElement.classList.add('income');
        amountElement.classList.remove('expense');
    } else {
        amountElement.textContent = `-$${decision.amount}`;
        amountElement.classList.remove('income');
        amountElement.classList.add('expense');
    }
    
    // Show actions
    document.querySelector('.card-actions').style.display = 'grid';
    document.querySelector('.card-category').style.display = 'block';
    document.querySelector('.card-amount').style.display = 'block';
}

// Make a decision
function makeDecision(choice) {
    const decision = gameState.monthlyDecisions[gameState.currentDecisionIndex];
    const impact = decision.impact[choice];
    
    // Apply impact
    gameState.balance += impact.balance;
    gameState.creditScore = Math.max(300, Math.min(850, gameState.creditScore + impact.credit));
    gameState.stressLevel = Math.max(0, Math.min(100, gameState.stressLevel + impact.stress));
    
    // Record decision
    const decisionRecord = {
        month: gameState.currentMonth,
        title: decision.title,
        choice: choice,
        amount: decision.amount,
        type: decision.type
    };
    
    gameState.totalDecisions.push(decisionRecord);
    
    // Add to decisions list UI
    addDecisionToList(decisionRecord);
    
    // Update UI
    updateGameUI();
    
    // Move to next decision
    gameState.currentDecisionIndex++;
    showNextDecision();
}

// Add decision to the list
function addDecisionToList(decision) {
    const list = document.getElementById('decisions-list');
    const item = document.createElement('div');
    item.className = 'decision-item';
    
    const choiceText = {
        pay: 'Paid',
        delay: 'Delayed',
        skip: 'Skipped'
    };
    
    item.innerHTML = `
        <div class="decision-info">
            <div class="decision-title">${decision.title}</div>
            <div class="decision-choice">${choiceText[decision.choice]}</div>
        </div>
        <div class="decision-amount ${decision.type === 'income' ? 'positive' : 'negative'}">
            ${decision.type === 'income' ? '+' : '-'}$${decision.amount}
        </div>
    `;
    
    list.appendChild(item);
}

// Update game UI
function updateGameUI() {
    document.getElementById('current-month').textContent = `${gameState.currentMonth}/${gameState.totalMonths}`;
    document.getElementById('balance').textContent = `$${gameState.balance.toLocaleString()}`;
    document.getElementById('credit-score').textContent = gameState.creditScore;
    document.getElementById('stress-level').textContent = `${Math.round(gameState.stressLevel)}%`;
    
    // Update colors based on values
    const balanceElement = document.getElementById('balance');
    if (gameState.balance < 0) {
        balanceElement.style.color = 'var(--danger)';
    } else if (gameState.balance < 500) {
        balanceElement.style.color = 'var(--warning)';
    } else {
        balanceElement.style.color = 'var(--success)';
    }
}

// Advance to next month
function nextMonth() {
    // Add monthly income
    let income = gameState.monthlyIncome;
    
    // Variable income for intermediate level
    if (gameState.level === 'intermediate') {
        income = 2800 + Math.floor(Math.random() * 700); // $2800-$3500
    }
    
    gameState.balance += income;
    
    // Check if game is over
    if (gameState.currentMonth >= gameState.totalMonths) {
        endGame();
        return;
    }
    
    // Move to next month
    gameState.currentMonth++;
    
    // Clear decisions list
    document.getElementById('decisions-list').innerHTML = '';
    
    // Generate new decisions
    generateMonthlyDecisions();
    
    // Update UI
    updateGameUI();
    
    // Show first decision
    showNextDecision();
    
    // Disable next month button
    document.querySelector('.next-month-button').disabled = true;
}

// End game
function endGame() {
    gameState.isGameOver = true;
    
    // Calculate final score
    const balanceScore = Math.max(0, gameState.balance * 0.3);
    const creditScore = (gameState.creditScore - 300) * 0.5;
    const stressScore = (100 - gameState.stressLevel) * 2;
    const finalScore = Math.round(balanceScore + creditScore + stressScore);
    
    // Update game over screen
    document.getElementById('final-balance').textContent = `$${gameState.balance.toLocaleString()}`;
    document.getElementById('final-credit').textContent = gameState.creditScore;
    document.getElementById('final-score').textContent = finalScore.toLocaleString();
    
    // Determine performance message
    let message = '';
    if (finalScore >= 1500) {
        document.getElementById('game-over-title').textContent = '🎉 Outstanding!';
        message = `Incredible work! You've mastered ${gameState.levelName} level with a score of ${finalScore}. Your financial decisions were excellent - you maintained a healthy balance, good credit, and low stress. You're ready for the next challenge!`;
    } else if (finalScore >= 1000) {
        document.getElementById('game-over-title').textContent = '✅ Well Done!';
        message = `Great job! You completed ${gameState.levelName} level with a score of ${finalScore}. You made solid financial decisions overall. There's room for improvement, but you're on the right track!`;
    } else if (finalScore >= 500) {
        document.getElementById('game-over-title').textContent = '👍 Not Bad!';
        message = `You finished ${gameState.levelName} level with a score of ${finalScore}. You survived, but it was tough. Try to balance paying bills with managing stress and maintaining your credit score better next time.`;
    } else {
        document.getElementById('game-over-title').textContent = '💪 Keep Trying!';
        message = `You completed ${gameState.levelName} level with a score of ${finalScore}. Budgeting is hard! Try again and focus on paying essential bills first, building an emergency fund, and avoiding unnecessary expenses.`;
    }
    
    document.getElementById('performance-message').textContent = message;
    
    // Show game over screen
    showGameOver();
    
    // Save high score to localStorage
    saveHighScore(finalScore);
}

// Save high score
function saveHighScore(score) {
    const key = `highscore_${gameState.level}`;
    const currentHigh = localStorage.getItem(key) || 0;
    
    if (score > currentHigh) {
        localStorage.setItem(key, score);
    }
}

// Initialize game on page load
document.addEventListener('DOMContentLoaded', function() {
    showLanding();
});
