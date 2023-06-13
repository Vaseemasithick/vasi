<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Card Listing</title>
  <style>
    /* Add your CSS styles here */
    .card-container {
      display: flex;
      flex-wrap: wrap;
    }
    
    .card {
      width: 300px;
      height: 200px;
      border: 1px solid #ccc;
      border-radius: 5px;
      padding: 10px;
      margin: 10px;
      position: relative;
      box-sizing: border-box;
    }
    
    .card .type {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 12px;
    }
    
    .card .name {
      font-weight: bold;
      margin-bottom: 10px;
    }
    
    .card .details {
      margin-bottom: 10px;
    }
    
    .card .details p {
      margin: 5px 0;
    }
    
    .tab {
      display: inline-block;
      padding: 5px 10px;
      margin-right: 10px;
      background-color: #ccc;
      cursor: pointer;
    }
    
    .tab.active {
      background-color: #999;
    }
    
    .filter-container {
      margin-top: 20px;
    }
    
    .filter-dropdown {
      padding: 5px 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      cursor: pointer;
    }
    
    .filter-dropdown:hover {
      background-color: #f0f0f0;
    }
  </style>
</head>
<body>
  <div class="tabs">
    <div class="tab active" onclick="showCards('Your')">Your</div>
    <div class="tab" onclick="showCards('all')">All</div>
    <div class="tab" onclick="showCards('blocked')">Blocked</div>
  </div>
  
  <div class="filter-container">
    <div class="filter-dropdown" onclick="toggleFilterDropdown()">Filter by Card Type ▼</div>
    <div class="filter-options" style="display: none;">
      <div onclick="filterByCardType('burner')">Burner</div>
      <div onclick="filterByCardType('subscription')">Subscription</div>
    </div>
  </div>
  
  <div id="cardListing"></div>
  
  <script>
    // Add your JavaScript code here
    const mockApiResponse = {
      data: [
        {
          name: 'Mixmax',
          budget_name: 'Software subscription',
          owner_id: 1,
          spent: {
            value: 100,
            currency: "SGD"
          },
          available_to_spend: {
            value: 1000,
            currency: "SGD"
          },
          card_type: "burner",
          expiry: "9 Feb",
          limit: 100,
          status: 'active'
        },
        {
          name: 'Quickbooks',
          budget_name: 'Software subscription',
          owner_id: 2,
          spent: {
            value: 50,
            currency: "SGD"
          },
          available_to_spend: {
            value: 250,
            currency: "SGD"
          },
          card_type: "subscription",
          limit: 10,
          status: 'active'
        }
      ],
      page: 1,
      per_page: 10,
      total: 100
    };
    
    let activeTab = 'Your';
    let activeFilter = '';
    
    function renderCards(cards) {
      const cardListing = document.getElementById('cardListing');
      cardListing.innerHTML = '';
      
      cards.forEach(card => {
        if (activeTab === 'Your' && card.owner_id !== 1) {
          return;
        }
        
        if (activeFilter && card.card_type !== activeFilter) {
          return;
        }
        
        const cardElement = document.createElement('div');
        cardElement.classList.add('card');
        
        const cardType = document.createElement('span');
        cardType.classList.add('type');
        cardType.innerText = card.card_type;
        
        const name = document.createElement('p');
        name.classList.add('name');
        name.innerText = card.name;
        
        const details = document.createElement('div');
        details.classList.add('details');
        
        if (card.card_type === 'burner') {
          const expiry = document.createElement('p');
          expiry.innerText = `Expiry: ${card.expiry}`;
          details.appendChild(expiry);
        } else if (card.card_type === 'subscription') {
          const limit = document.createElement('p');
          limit.innerText = `Limit: ${card.limit}`;
          details.appendChild(limit);
        }
        
        // Add other card details as per your design
        
        cardElement.appendChild(cardType);
        cardElement.appendChild(name);
        cardElement.appendChild(details);
        
        cardListing.appendChild(cardElement);
      });
    }
    
    function showCards(tab) {
      const tabs = document.getElementsByClassName('tab');
      for (let i = 0; i < tabs.length; i++) {
        tabs[i].classList.remove('active');
      }
      
      const cardListing = document.getElementById('cardListing');
      cardListing.innerHTML = '';
      
      activeTab = tab;
      
      if (tab === 'Your') {
        renderCards(mockApiResponse.data);
      } else if (tab === 'all') {
        // Fetch data from API for all cards
        // renderCards(allCardsData);
      } else if (tab === 'blocked') {
        // Fetch data from API for blocked cards
        // renderCards(blockedCardsData);
      }
      
      event.currentTarget.classList.add('active');
    }
    
    function toggleFilterDropdown() {
      const filterOptions = document.querySelector('.filter-options');
      filterOptions.style.display = filterOptions.style.display === 'none' ? 'block' : 'none';
    }
    
    function filterByCardType(type) {
      const filterDropdown = document.querySelector('.filter-dropdown');
      filterDropdown.innerText = `Filter by Card Type ▼ (${type})`;
      
      const filterOptions = document.querySelector('.filter-options');
      filterOptions.style.display = 'none';
      
      activeFilter = type;
      
      if (activeTab === 'Your') {
        renderCards(mockApiResponse.data);
      } else if (activeTab === 'all') {
        // Fetch data from API for all cards
        // renderCards(allCardsData);
      } else if (activeTab === 'blocked') {
        // Fetch data from API for blocked cards
        // renderCards(blockedCardsData);
      }
    }
    
    showCards('Your'); // Initial rendering
    
    // Implement infinite scroll to fetch more data
    
    // Implement search by card name
  </script>
</body>
</html>
