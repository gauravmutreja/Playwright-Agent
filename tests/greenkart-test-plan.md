# GreenKart Vegetable E-Commerce & Flight Booking Application Test Plan

## Application Overview

This is a comprehensive test plan for the GreenKart application (https://rahulshettyacademy.com/seleniumPractise/#/), which is an e-commerce platform for buying vegetables and fruits with an integrated flight booking system. The application consists of three main sections: (1) Home page with product catalog and shopping cart functionality, (2) Top Deals page showing discounted products in a table format with pagination, and (3) Flight Booking page for searching and booking flights. The test plan covers core functionality including product browsing, search, cart management, discount offers, and flight booking operations.

## Test Scenarios

### 1. Home Page - Product Catalog & Shopping Cart

**Seed:** `tests/seed.spec.ts`

#### 1.1. Verify product catalog displays correctly

**File:** `tests/home-page/product-catalog.spec.ts`

**Steps:**
  1. Navigate to the GreenKart home page
    - expect: Page loads successfully
    - expect: Product grid is visible
    - expect: Multiple vegetable products are displayed (Broccoli, Cauliflower, Cucumber, Beetroot, Carrot, Tomato, Green Beans, Eggplant, etc.)
  2. Observe product card structure
    - expect: Each product shows: product image
    - expect: Product name with weight/quantity
    - expect: Price in Indian Rupees (₹)
    - expect: Quantity adjustment buttons (- and +)
    - expect: ADD TO CART button
  3. Verify header and navigation elements
    - expect: GREENKART logo is visible
    - expect: Search bar is present with placeholder 'Search for Vegetables and Fruits'
    - expect: Top Deals link is visible
    - expect: Flight Booking link is visible
    - expect: Shopping cart icon with item count (initially 0)
    - expect: Price display in cart (initially ₹0)
  4. Verify page footer
    - expect: Footer displays '© 2019 GreenKart - buy veg and fruits online'

#### 1.2. Add single item to cart

**File:** `tests/home-page/add-to-cart.spec.ts`

**Steps:**
  1. Load the home page with fresh cart (Items: 0, Price: ₹0)
    - expect: Page displays all products
    - expect: Cart counter shows 0
    - expect: Total price shows ₹0
  2. Click ADD TO CART button for the first product (Broccoli - ₹120)
    - expect: Item is added to cart
    - expect: Cart counter updates to 1
    - expect: Total price updates to ₹120
  3. Verify product card for Broccoli
    - expect: Quantity selector shows 1
    - expect: Product card may indicate item is in cart

#### 1.3. Add multiple items to cart with quantity adjustment

**File:** `tests/home-page/add-multiple-items.spec.ts`

**Steps:**
  1. Navigate to home page with empty cart
    - expect: Cart is empty (Items: 0, Price: ₹0)
  2. For Broccoli (₹120): Click the + button twice to increase quantity to 3
    - expect: Quantity field shows 3
  3. Click ADD TO CART for Broccoli with quantity 3
    - expect: Cart counter updates to 3
    - expect: Total price updates to ₹360 (3 × 120)
  4. For Cauliflower (₹60): Leave quantity at 1 and click ADD TO CART
    - expect: Cart counter updates to 4 (total items)
    - expect: Total price updates to ₹420 (360 + 60)
  5. For Cucumber (₹48): Click + once to set quantity to 2 and add to cart
    - expect: Cart counter updates to 6 (total items)
    - expect: Total price updates to ₹516 (420 + 96)

#### 1.4. Adjust quantity using +/- buttons

**File:** `tests/home-page/quantity-adjustment.spec.ts`

**Steps:**
  1. Navigate to home page and locate a product card (e.g., Broccoli)
    - expect: Product card is visible
    - expect: Quantity field shows 1 by default
  2. Click + button multiple times (5 times)
    - expect: Quantity increments with each click
    - expect: Final quantity displays as 6
  3. Click - button multiple times (3 times)
    - expect: Quantity decrements with each click
    - expect: Final quantity displays as 3
  4. Click - button when quantity is 1
    - expect: Quantity stays at 1 (cannot go below 1)
    - expect: No negative values are displayed

#### 1.5. Search for existing product

**File:** `tests/home-page/search-existing.spec.ts`

**Steps:**
  1. Navigate to home page
    - expect: All products are displayed
  2. Click on the search box and type 'carrot'
    - expect: Search input accepts the text
    - expect: Products matching 'carrot' are filtered and displayed
  3. Verify search results
    - expect: Only Carrot product is shown
    - expect: Other products are hidden
  4. Clear the search box
    - expect: All products display again

#### 1.6. Search for non-existent product

**File:** `tests/home-page/search-nonexistent.spec.ts`

**Steps:**
  1. Navigate to home page
    - expect: All products are displayed
  2. Click on the search box and type 'xyz' (non-existent product)
    - expect: Search input accepts the text
  3. Verify search results
    - expect: Empty state message displays: 'Sorry, no products matched your search!'
    - expect: An empty tree image is displayed
    - expect: Suggestion text appears: 'Enter a different keyword and try.'

#### 1.7. Use search with special characters and case sensitivity

**File:** `tests/home-page/search-edge-cases.spec.ts`

**Steps:**
  1. Test search with uppercase letters (e.g., 'POTATO')
    - expect: Search should either be case-insensitive and show Potato
    - expect: Or show no results if case-sensitive
  2. Test search with partial product names (e.g., 'bro' for Broccoli)
    - expect: Products matching partial text are shown
    - expect: Or only exact matches are shown (behavior to be documented)
  3. Test search with numbers (e.g., '1 kg')
    - expect: Search either filters by weight or shows no results

#### 1.8. View cart summary and proceed to checkout

**File:** `tests/home-page/cart-operations.spec.ts`

**Steps:**
  1. Add 2 items to cart (e.g., Broccoli with quantity 2, Carrot with quantity 1)
    - expect: Cart counter shows 3 total items
    - expect: Cart price is calculated correctly
  2. Click on the Shopping Cart icon
    - expect: Cart details page or modal appears
    - expect: Cart displays all added items with quantities
    - expect: Total price is displayed
  3. Verify PROCEED TO CHECKOUT button presence
    - expect: PROCEED TO CHECKOUT button is visible

### 2. Top Deals - Discount Products & Pagination

**Seed:** `tests/seed.spec.ts`

#### 2.1. Navigate to Top Deals and verify table structure

**File:** `tests/top-deals/navigate-top-deals.spec.ts`

**Steps:**
  1. Click on 'Top Deals' link in the navigation
    - expect: Top Deals page loads successfully
    - expect: URL changes to '#/offers'
    - expect: A table is displayed with product offers
  2. Verify table headers
    - expect: Column headers are visible: 'Veg/fruit name', 'Price', 'Discount price'
    - expect: Headers are sortable (clickable with sort indicators)
  3. Verify table data
    - expect: At least 5 products are displayed (Wheat, Tomato, Strawberry, Rice, Potato)
    - expect: Each product shows original price and discount price
    - expect: Discount prices are lower than original prices

#### 2.2. Test pagination functionality

**File:** `tests/top-deals/pagination.spec.ts`

**Steps:**
  1. Load Top Deals page with default page size (5)
    - expect: First 5 products are displayed
    - expect: Page 1 is marked as active
    - expect: First and Previous buttons are disabled
  2. Click on 'Next' button
    - expect: Page 2 is loaded
    - expect: Next set of 5 products is displayed
    - expect: Active page indicator moves to 2
  3. Click on page number 4
    - expect: Page 4 is loaded
    - expect: Products for page 4 are displayed
  4. Click on 'Previous' button
    - expect: Page 3 is loaded
    - expect: Products for page 3 are displayed
  5. Click on 'Last' button
    - expect: Last page is loaded
    - expect: Last page products are displayed
    - expect: Next and Last buttons are disabled
  6. Click on 'First' button
    - expect: First page is loaded
    - expect: First page products are displayed

#### 2.3. Test page size selector

**File:** `tests/top-deals/page-size.spec.ts`

**Steps:**
  1. Load Top Deals page with default page size 5
    - expect: 5 products are displayed per page
  2. Click on page size dropdown and select 10
    - expect: Page refreshes showing 10 products per page
    - expect: Pagination updates accordingly
  3. Click on page size dropdown and select 20
    - expect: Page refreshes showing 20 products per page
  4. Select back to 5
    - expect: 5 products are displayed per page again

#### 2.4. Test search in Top Deals

**File:** `tests/top-deals/search.spec.ts`

**Steps:**
  1. Load Top Deals page
    - expect: All products are displayed in table
  2. Type 'Potato' in the search box
    - expect: Table filters to show only Potato
    - expect: Other products are hidden
  3. Clear the search box
    - expect: All products display again in the table
  4. Search for non-existent product (e.g., 'xyz')
    - expect: Table shows no results or empty state

#### 2.5. Test column sorting

**File:** `tests/top-deals/sorting.spec.ts`

**Steps:**
  1. Load Top Deals page (default sorted by name descending)
    - expect: Alert message shows 'Sorted by name: descending order'
    - expect: Products are listed in descending alphabetical order
  2. Click on 'Veg/fruit name' column header
    - expect: Products are sorted in ascending alphabetical order
  3. Click on 'Price' column header
    - expect: Products are sorted by price
  4. Click on 'Discount price' column header
    - expect: Products are sorted by discount price

#### 2.6. Verify delivery date selector

**File:** `tests/top-deals/delivery-date.spec.ts`

**Steps:**
  1. Load Top Deals page
    - expect: Delivery Date section is visible at the bottom
  2. Observe the date picker showing current date (02/22/2026)
    - expect: Date picker displays in MM/DD/YYYY format
    - expect: Date fields for day, month, year are visible
  3. Modify the delivery date by clicking on date fields and changing values
    - expect: Date fields accept user input
    - expect: Calendar navigation buttons are present

### 3. Flight Booking - Flight Search & Reservation

**Seed:** `tests/seed.spec.ts`

#### 3.1. Navigate to Flight Booking and verify page structure

**File:** `tests/flight-booking/navigate-flight-booking.spec.ts`

**Steps:**
  1. Click on 'Flight Booking' link or navigate to https://rahulshettyacademy.com/dropdownsPractise/
    - expect: Flight Booking page loads successfully
    - expect: Page displays a comprehensive flight search form
  2. Verify main navigation elements
    - expect: Page shows links for: Flights, Hotels, Holiday Packages, Flight Status, Check-In, Manage Booking
  3. Verify the country selector
    - expect: A country dropdown/textbox is present
    - expect: Can type to select country

#### 3.2. Test trip type selection

**File:** `tests/flight-booking/trip-type.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: 'One Way' option is selected by default
    - expect: Three trip type options visible: One Way, Round Trip, Multicity
  2. Click on 'Round Trip' radio button
    - expect: Round Trip is selected
    - expect: Return date field becomes visible/enabled
  3. Click on 'Multicity' radio button
    - expect: Multicity is selected
    - expect: Form may adapt to show multiple trip segments
  4. Select 'One Way' again
    - expect: One Way is selected
    - expect: Return date field is disabled/hidden

#### 3.3. Test departure and arrival city selection

**File:** `tests/flight-booking/city-selection.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: FROM field (Departure City) is visible and required
    - expect: TO field (Arrival City) is visible and required
    - expect: Both fields display placeholder text
  2. Click on FROM field and type a city name
    - expect: City suggestions appear (if auto-complete is available)
    - expect: User can select or type a city
  3. Click on TO field and type a different city
    - expect: Destination city is selected
  4. Try to search without selecting FROM or TO
    - expect: Validation error appears or search is prevented

#### 3.4. Test date selection for departure

**File:** `tests/flight-booking/departure-date.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: Depart date field shows current/default date (05/05)
    - expect: Depart date is required
  2. Click on the depart date field or calendar icon
    - expect: Date picker opens
    - expect: Current date or default date is highlighted
  3. Select a future date
    - expect: Selected date is populated in the field
    - expect: Date format is maintained

#### 3.5. Test passenger selection

**File:** `tests/flight-booking/passengers.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: Passengers field shows '1 Adult' by default
  2. Click on passengers dropdown
    - expect: Passenger selection options appear
    - expect: Can select number of adults, children, infants
  3. Select '2 Adults'
    - expect: Passengers field updates to show '2 Adults'
  4. Add children to the selection
    - expect: Field updates to show both adults and children

#### 3.6. Test currency selection

**File:** `tests/flight-booking/currency.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: Currency section is visible
    - expect: A dropdown showing currency options is present
  2. Verify default currency selection
    - expect: INR (Indian Rupees) is selected by default
  3. Click on currency dropdown and view options
    - expect: Options available: Select, INR, AED, USD
  4. Select USD
    - expect: Currency changes to USD
    - expect: Prices on search results should reflect USD
  5. Select AED
    - expect: Currency changes to AED

#### 3.7. Test discount/category checkboxes

**File:** `tests/flight-booking/discounts.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: Discount/category section displays checkboxes
    - expect: Options: Family and Friends, Senior Citizen, Indian Armed Forces, Student, Unaccompanied Minor
  2. Check 'Senior Citizen' checkbox
    - expect: Checkbox is marked
    - expect: May apply senior citizen discount to search results
  3. Check 'Student' checkbox
    - expect: Checkbox is marked
    - expect: Multiple checkboxes can be selected simultaneously
  4. Uncheck 'Senior Citizen'
    - expect: Checkbox is unchecked
  5. Verify all other checkboxes can be toggled
    - expect: All checkboxes are functional

#### 3.8. Test Special Assistance link

**File:** `tests/flight-booking/special-assistance.spec.ts`

**Steps:**
  1. Load Flight Booking page
    - expect: 'Special Assistance' link is visible
  2. Click on 'Special Assistance' link
    - expect: Page navigates to special assistance section or dialog appears
    - expect: Special needs options are displayed

#### 3.9. Test complete flight search workflow

**File:** `tests/flight-booking/complete-search.spec.ts`

**Steps:**
  1. Load Flight Booking page with default One Way trip type
    - expect: Form is ready for input
  2. Select 'Round Trip' option
    - expect: Return date field becomes available
  3. Enter departure city and arrival city
    - expect: Cities are entered in the form
  4. Select departure date from calendar
    - expect: Departure date is set
  5. Select return date (since Round Trip is selected)
    - expect: Return date is set
  6. Select 2 passengers (1 Adult, 1 Child)
    - expect: Passenger count is updated
  7. Change currency to USD
    - expect: Currency is changed
  8. Select 'Senior Citizen' discount
    - expect: Senior citizen discount is applied
  9. Click 'Search' button
    - expect: Flight search is initiated
    - expect: Search results should load with matching flights
