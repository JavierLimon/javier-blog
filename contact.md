---
layout: page
title: Contact
ref: contact
order: 1
---
<link href="assets/css/contact-form.css" rel="stylesheet">

<div class="fcf-body">

    <div id="fcf-form">
    <h3 class="fcf-h3">Contact</h3>

    <form id="fcf-form-id" class="fcf-form-class" action="https://getform.io/f/3bad303b-896a-4ad2-b858-eb4b80511aec" method="POST">

        <div class="fcf-form-group">
            <label for="Name" class="fcf-label">Name</label>
            <div class="fcf-input-group">
                <input type="text" name="name" style="width: 820px;" class="fcf-form-control" required>
            </div>
        </div>

        <div class="fcf-form-group">
            <label for="Email" class="fcf-label">Email address</label>
            <div class="fcf-input-group">
                <input type="email" name="email" style="width: 820px;" class="fcf-form-control" required>
            </div>
        </div>

        <div class="fcf-form-group">
            <label for="Message" class="fcf-label">Message</label>
            <div class="fcf-input-group">
                <textarea name="message" class="fcf-form-control" style="width: 820px;" rows="6" maxlength="3000" required></textarea>
            </div>
        </div>
        <br/>
        <div class="fcf-form-group">
            <button type="submit" id="fcf-button" class="fcf-btn fcf-btn-primary fcf-btn-lg fcf-btn-block">Send Message</button>
        </div>
    </form>
    </div>

</div>
