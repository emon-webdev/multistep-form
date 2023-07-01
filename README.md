# multistep-form
multistep-form using javaScript


//



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multi-Step Form</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">

    <style>
        /* Add your CSS styles here */
        .hidden {
            display: none;
        }

        .error {
            color: red;
        }

        /* Add your animation styles here */
        .step {
            animation: slide-in 0.5s ease-in-out forwards;
        }

        @keyframes slide-in {
            0% {
                opacity: 0;
                transform: translateY(50px);
            }
            100% {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>
<body>
    <div class="multistep-contact-form">
        <form id="contactForm" action="submit-form.php" method="POST">
            <fieldset class="step">
                <div class="mb-3">
                    <label for="what_are_you_looking_for" class="form-label">What are you looking for? *</label>
                    <div class="form-check">
                        <input class="form-check-input" type="radio" name="what_are_you_looking_for" id="roof_replacement">
                        <label class="form-check-label" for="roof_replacement">Roof Replacement</label>
                    </div>
                    <div class="form-check">
                        <input class="form-check-input" type="radio" name="what_are_you_looking_for" id="roof_repair" checked>
                        <label class="form-check-label" for="roof_repair">Roof Repair</label>
                    </div>
                    <div class="form-check">
                        <input class="form-check-input" type="radio" name="what_are_you_looking_for" id="not_sure_free_inspection">
                        <label class="form-check-label" for="not_sure_free_inspection">Not sure. Free inspection.</label>
                    </div>
                </div>
                <button type="button" class="next-button">CONTINUE</button>
            </fieldset>

            <fieldset class="step hidden">
                <div class="mb-3">
                    <label for="your_location" class="form-label">Your Location *</label>
                    <input type="text" class="form-control" id="your_location" name="your_location">
                </div>
                <button type="button" class="next-button">CONTINUE</button>
            </fieldset>

            <fieldset class="step hidden">
                <div class="mb-3">
                    <label for="first_name" class="form-label">First Name *</label>
                    <input type="text" class="form-control" id="first_name" name="first_name">
                </div>
                <button type="button" class="next-button">CONTINUE</button>
            </fieldset>

            <fieldset class="step hidden">
                <div class="mb-3">
                    <label for="your_email" class="form-label">Your Address *</label>
                    <input type="email" class="form-control" id="your_email" name="your_email">
                </div>
                <button type="button" class="next-button">CONTINUE</button>
            </fieldset>

            <fieldset class="step hidden">
                <div class="mb-3">
                    <label for="your_phone" class="form-label">Your Phone *</label>
                    <input type="text" class="form-control" id="your_phone" name="your_phone">
                </div>
                <button type="submit" class="submit-button">SUBMIT</button>
            </fieldset>
        </form>
    </div>

    <!-- Add your Bootstrap modal code here -->
    <div class="modal" id="successModal" tabindex="-1" role="dialog">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Form Submitted Successfully!</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    Thank you for submitting the form.
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-geWF76RCwLtnZ8qwWowPQNguL3RmwHVBC9FhGdlKrxdiJJigb/j/68SIy3Te4Bkz" crossorigin="anonymous"></script>

    <script>
        // Add your JavaScript code here

        
        const steps = document.querySelectorAll('.step');
        const nextButton = document.querySelectorAll('.next-button');
        const submitButton = document.querySelector('.submit-button');
        const form = document.getElementById('contactForm');
        const successModal = document.getElementById('successModal');
    
        let currentStep = 0;
    
        function showStep(stepIndex) {
            steps[currentStep].classList.add('hidden');
            steps[stepIndex].classList.remove('hidden');
            steps[stepIndex].classList.add('step');
            currentStep = stepIndex;
    
            if (currentStep === steps.length - 1) {
                submitButton.textContent = 'SUBMIT';
            } else {
                submitButton.textContent = 'CONTINUE';
            }
        }
    
        function goToNextStep() {
            if (validateForm(currentStep)) {
                showStep(currentStep + 1);
            }
        }
    
        function validateForm(stepIndex) {
            const inputFields = Array.from(steps[stepIndex].querySelectorAll('input'));
    
            for (let i = 0; i < inputFields.length; i++) {
                if (inputFields[i].value === '') {
                    inputFields[i].classList.add('error');
                    return false;
                } else {
                    inputFields[i].classList.remove('error');
                }
            }
    
            // Perform additional form validation if needed
    
            return true;
        }
    
        function handleFormSubmit(event) {
            event.preventDefault();
    
            if (validateForm(currentStep)) {
                // Perform additional form validation if needed
    
                // Submit the form using AJAX or other methods
                submitForm();
            }
        }
    
        function submitForm() {
            // Prepare form data to be sent
            const formData = new FormData(form);

            // Get the selected radio button value
            const selectedRadioButton = document.querySelector('input[name="what_are_you_looking_for"]:checked');
            formData.append('what_are_you_looking_for', selectedRadioButton.value);

            // Create an AJAX request
            const xhr = new XMLHttpRequest();
            xhr.open('POST', form.action);
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

            // Handle the AJAX response
            xhr.onload = function () {
                if (xhr.status === 200) {
                const response = xhr.responseText;
                if (response.trim() === 'success') {
                    // Display success modal
                    showSuccessModal();

                    // Reset the form and go to the first field
                    form.reset();
                    showStep(0);
                } else {
                    // Handle server-side validation errors or other failures
                    console.log('Oops! Something went wrong. Please try again later.');
                }
                } else {
                // Handle AJAX request errors
                console.log('Oops! Something went wrong. Please try again later.');
                }
            };

            // Send the AJAX request
            xhr.send(new URLSearchParams(formData));
        }

        function showSuccessModal() {
            successModal.style.display = 'block';

             // Add event listener to close button
            const closeButton = successModal.querySelector('.close');
            closeButton.addEventListener('click', hideSuccessModal);
        }
    
        // Hide the success modal initially
        function hideSuccessModal() {
            successModal.style.display = 'none';
        }
    
        // Add event listeners for next and submit buttons
        nextButton.forEach(button => {
            button.addEventListener('click', goToNextStep);
        });
    
        form.addEventListener('submit', handleFormSubmit);
    </script>
    
    
    
</body>
</html>
