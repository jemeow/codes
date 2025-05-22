# @ -1,5 +1,6 @@
// Event Listeners
document.addEventListener('DOMContentLoaded', () => {
<<<<<<< Updated upstream:SOURCE CODE/SystemDesign/js/manageEmployee.js
    // Check if RBAC service is available and enforce access
    if (typeof RBACService !== 'undefined') {
        // Enforce that only admin can access this page
@ -15,24 +16,91 @@ document.addEventListener('DOMContentLoaded', () => {
    
    // Initialize page
    loadEmployees();
=======
>>>>>>> Stashed changes:SOURCE CODE/SystemDesign/manageEmployee.js
    setupEventListeners();
    setupContactValidation();
    loadEmployees();
});

let employeeToDelete = null;

function setupEventListeners() {
    // Logout button
    document.querySelector('.logout-btn').addEventListener('click', handleLogout);
    const addBtn = document.getElementById('addEmployeeBtn');
    const popup = document.getElementById('addEmployeePopup');
    const saveBtn = document.getElementById('saveBtn');
    const cancelBtn = document.getElementById('cancelBtn');
    const searchInput = document.querySelector('.search-input');

    addBtn.addEventListener('click', () => {
        popup.style.display = 'flex';
        document.getElementById('employeeForm').reset();
    });

    cancelBtn.addEventListener('click', () => {
        popup.style.display = 'none';
    });

    saveBtn.addEventListener('click', handleSaveEmployee);

    searchInput.addEventListener('input', (e) => {
        filterEmployees(e.target.value.toLowerCase());
    });

    document.querySelector('.logout-btn').addEventListener('click', () => {
        if(confirm('Are you sure you want to logout?')) {
            window.location.href = 'login.html';
        }
    });

    // Image upload handling
    const dropZone = document.querySelector('.drop-zone');
    const fileInput = document.querySelector('.drop-zone__input');

    dropZone.addEventListener('click', () => fileInput.click());
    
    // Add employee button
    document.getElementById('addEmployeeBtn').addEventListener('click', () => {
        // Add employee logic
    fileInput.addEventListener('change', (e) => {
        if (e.target.files.length) {
            const file = e.target.files[0];
            if (validateImageFile(file)) {
                handleImageUpload(file);
            }
        }
    });

    // Search input
    document.getElementById('searchInput').addEventListener('input', (e) => {
        filterEmployees(e.target.value);
    const editPopup = document.getElementById('editEmployeePopup');
    const updateBtn = document.getElementById('updateBtn');
    const editCancelBtn = document.getElementById('editCancelBtn');

    editCancelBtn.addEventListener('click', () => {
        editPopup.style.display = 'none';
    });

    updateBtn.addEventListener('click', handleUpdateEmployee);

    const cancelDeleteBtn = document.getElementById('cancelDeleteBtn');
    const confirmDeleteBtn = document.getElementById('confirmDeleteBtn');

    cancelDeleteBtn.addEventListener('click', () => {
        document.getElementById('deleteConfirmPopup').style.display = 'none';
        employeeToDelete = null;
    });

    confirmDeleteBtn.addEventListener('click', () => {
        if (employeeToDelete) {
            const index = employees.findIndex(emp => emp.userId === employeeToDelete);
            if (index !== -1) {
                employees.splice(index, 1);
                displayEmployees(employees);
                document.getElementById('deleteConfirmPopup').style.display = 'none';
                document.getElementById('deleteSuccessPopup').style.display = 'flex';
                employeeToDelete = null;
            }
        }
    });
}

<<<<<<< Updated upstream:SOURCE CODE/SystemDesign/js/manageEmployee.js
function handleLogout() {
    // Use the shared logout helper if available
    if (typeof window.handleLogout === 'function') {
@ -63,7 +131,119 @@ function handleLogout() {
                window.location.href = 'pages/loginInterface.html';
            }
        });
=======
function validateImageFile(file) {
    const validTypes = ['image/jpeg', 'image/png', 'image/gif'];
    const maxSize = 5 * 1024 * 1024; // 5MB

    if (!validTypes.includes(file.type)) {
        alert('Please upload a JPEG, PNG, or GIF image');
        return false;
>>>>>>> Stashed changes:SOURCE CODE/SystemDesign/manageEmployee.js
    }

    if (file.size > maxSize) {
        alert('File size should be less than 5MB');
        return false;
    }

    return true;
}

function handleImageUpload(file) {
    const dropZone = document.querySelector('.drop-zone');
    const preview = dropZone.querySelector('.image-preview');
    const prompt = dropZone.querySelector('.drop-zone__prompt');
    
    const reader = new FileReader();
    reader.onload = (e) => {
        preview.innerHTML = `<img src="${e.target.result}" alt="Selected Image">`;
        preview.dataset.imageData = e.target.result;
        preview.style.display = 'block';
        prompt.style.display = 'none';
        dropZone.classList.add('has-image');
    };
    reader.readAsDataURL(file);
}

function resetImageUpload() {
    const dropZone = document.querySelector('.drop-zone');
    const preview = dropZone.querySelector('.image-preview');
    const prompt = dropZone.querySelector('.drop-zone__prompt');
    
    preview.innerHTML = '';
    preview.style.display = 'none';
    prompt.style.display = 'block';
    prompt.textContent = 'Click to upload';
    dropZone.classList.remove('has-image');
    document.querySelector('.drop-zone__input').value = '';
}

function setupContactValidation() {
    const contactInput = document.getElementById('contact');
    
    contactInput.addEventListener('input', (e) => {
        // Remove any non-numeric characters
        e.target.value = e.target.value.replace(/[^0-9]/g, '');
        
        // Ensure maximum length of 11 digits
        if (e.target.value.length > 11) {
            e.target.value = e.target.value.slice(0, 11);
        }
    });

    contactInput.addEventListener('keypress', (e) => {
        // Allow only numeric input
        if (!/[0-9]/.test(e.key)) {
            e.preventDefault();
        }
    });
}

function handleSaveEmployee() {
    const empId = document.getElementById('userId').value;
    const firstName = document.getElementById('firstName').value;
    const middleName = document.getElementById('middleName').value;
    const lastName = document.getElementById('lastName').value;
    const birthday = document.getElementById('birthday').value;
    const contact = document.getElementById('contact').value;
    const email = document.getElementById('email').value;
    const role = document.getElementById('role').value;

    // Validate fields
    if (!empId || !firstName || !lastName || !birthday || !contact || !email || !role) {
        alert('Please fill in all required fields');
        return;
    }

    const preview = document.querySelector('.image-preview');
    const imageData = preview.dataset.imageData || null;

    // Create employee object
    const employeeData = {
        userId: empId,
        firstName,
        middleName,
        lastName,
        birthday,
        contact,
        email,
        role,
        status: 'Active',
        image: imageData
    };

    // Add to employees array
    employees.push(employeeData);
    displayEmployees(employees);
    
    // Reset form and close popup
    document.getElementById('addEmployeePopup').style.display = 'none';
    document.getElementById('employeeForm').reset();
    resetImageUpload();

    // Show success popup
    document.getElementById('addSuccessPopup').style.display = 'flex';
}

function loadEmployees() {
@ -74,21 +254,23 @@ function loadEmployees() {
    displayEmployees(employees);
}

function displayEmployees(employees) {
    const tbody = document.getElementById('employeeTableBody');
function displayEmployees(employeesList) {
    const tbody = document.querySelector('.employees-table tbody');
    tbody.innerHTML = '';

    employees.forEach(emp => {
    employeesList.forEach(emp => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${emp.userId}</td>
            <td>${emp.name}</td>
            <td>${emp.firstName} ${emp.middleName} ${emp.lastName}</td>
            <td>${emp.email}</td>
            <td>${emp.role}</td>
            <td>${emp.status}</td>
            <td>
                <button class="edit-btn" onclick="editEmployee(${emp.userId})">Edit</button>
                <button class="delete-btn" onclick="deleteEmployee(${emp.userId})">Delete</button>
                <div class="action-buttons">
                    <button class="edit-btn" onclick="editEmployee('${emp.userId}')">Edit</button>
                    <button class="delete-btn" onclick="deleteEmployee('${emp.userId}')">Delete</button>
                </div>
            </td>
        `;
        tbody.appendChild(row);
@ -100,9 +282,75 @@ function filterEmployees(searchTerm) {
}

function editEmployee(userId) {
    // Implement edit functionality
    const employee = employees.find(emp => emp.userId === userId);
    if (!employee) return;

    // Populate edit form
    document.getElementById('editUserId').value = employee.userId;
    document.getElementById('editFirstName').value = employee.firstName;
    document.getElementById('editMiddleName').value = employee.middleName;
    document.getElementById('editLastName').value = employee.lastName;
    document.getElementById('editBirthday').value = employee.birthday;
    document.getElementById('editContact').value = employee.contact;
    document.getElementById('editEmail').value = employee.email;
    document.getElementById('editRole').value = employee.role;

    // Show employee image if exists
    if (employee.image) {
        const preview = document.querySelector('#editEmployeePopup .image-preview');
        preview.innerHTML = `<img src="${employee.image}" alt="Employee Image">`;
        preview.style.display = 'block';
        preview.dataset.imageData = employee.image;
        document.querySelector('#editEmployeePopup .drop-zone__prompt').style.display = 'none';
        document.querySelector('#editEmployeePopup .drop-zone').classList.add('has-image');
    }

    // Show edit popup
    document.getElementById('editEmployeePopup').style.display = 'flex';
}

function handleUpdateEmployee() {
    const userId = document.getElementById('editUserId').value;
    const updatedData = {
        userId,
        firstName: document.getElementById('editFirstName').value,
        middleName: document.getElementById('editMiddleName').value,
        lastName: document.getElementById('editLastName').value,
        birthday: document.getElementById('editBirthday').value,
        contact: document.getElementById('editContact').value,
        email: document.getElementById('editEmail').value,
        role: document.getElementById('editRole').value,
        status: 'Active',
        image: document.querySelector('#editEmployeePopup .image-preview').dataset.imageData
    };

    // Validate fields
    if (!updatedData.firstName || !updatedData.lastName || !updatedData.birthday || 
        !updatedData.contact || !updatedData.email || !updatedData.role) {
        alert('Please fill in all required fields');
        return;
    }

    // Update employee in array
    const index = employees.findIndex(emp => emp.userId === userId);
    if (index !== -1) {
        employees[index] = updatedData;
        displayEmployees(employees);
        document.getElementById('editEmployeePopup').style.display = 'none';
        
        // Show success popup
        document.getElementById('updateSuccessPopup').style.display = 'flex';
    }
}

function closeSuccessPopup(popupId) {
    document.getElementById(popupId).style.display = 'none';
}

function deleteEmployee(userId) {
    // Implement delete functionality
    employeeToDelete = userId;
    document.getElementById('deleteConfirmPopup').style.display = 'flex';
}

// Sample data array (replace with your database)
let employees = [];
