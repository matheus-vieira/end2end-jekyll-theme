---
layout: post
title: "A test post with all form elements"
date: 2026-05-20 11:28 -0300
comments: true
image: https://jekyllrb.com/img/octojekyll.png
categories: [jekyll, update]
---
This is just a test post to verify that all form elements are rendered correctly in the generated site.

This form is not meant to be functional, but it includes examples of all standard HTML form elements for testing purposes.

> Demo note: this form is intentionally non-functional and is here to exercise the theme's form styles and browser defaults.

Fill out the form below to see how each element is displayed on the site.

This text is just filler content to provide context for the form elements. You can ignore it and focus on the form itself.

Let's start testing the form elements!

Just now.

## Form Example

<form method="POST" action="#">
  
  <div>
    <label for="fullname">Full Name (Text):</label>
    <input type="text" id="fullname" name="fullname" required>
  </div>

  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
  </div>

  <div>
    <label for="age">Age (Number):</label>
    <input type="number" id="age" name="age" min="0" max="120">
  </div>

  <div>
    <label for="phone">Phone:</label>
    <input type="tel" id="phone" name="phone" placeholder="(123) 456-7890">
  </div>

  <div>
    <label for="website">Website (URL):</label>
    <input type="url" id="website" name="website" placeholder="https://example.com">
  </div>

  <div>
    <label for="birthdate">Birth Date:</label>
    <input type="date" id="birthdate" name="birthdate">
  </div>

  <div>
    <label for="time">Preferred Time:</label>
    <input type="time" id="time" name="time">
  </div>

  <div>
    <label for="color">Favorite Color:</label>
    <input type="color" id="color" name="color">
  </div>

  <div>
    <label for="search">Search:</label>
    <input type="search" id="search" name="search" placeholder="Search...">
  </div>

  <div>
    <label for="range">Satisfaction (Range 1-10):</label>
    <input type="range" id="range" name="range" min="1" max="10">
  </div>

  <div>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
  </div>

  <div>
    <label for="country">Country (Select):</label>
    <select id="country" name="country">
      <option value="">-- Select --</option>
      <option value="br">Brazil</option>
      <option value="us">United States</option>
      <option value="mx">Mexico</option>
      <option value="other">Other</option>
    </select>
  </div>

  <div>
    <label for="message">Message (Textarea):</label>
    <textarea id="message" name="message" rows="4" cols="50" placeholder="Enter your message here..."></textarea>
  </div>

  <div>
    <label>
      <input type="checkbox" name="subscribe" value="yes">
      Subscribe to newsletter
    </label>
  </div>

  <div>
    <label>
      <input type="checkbox" name="terms" value="yes" required>
      I agree to the terms and conditions
    </label>
  </div>

  <div>
    <label>Gender (Radio):</label>
    <label>
      <input type="radio" name="gender" value="male">
      Male
    </label>
    <label>
      <input type="radio" name="gender" value="female">
      Female
    </label>
    <label>
      <input type="radio" name="gender" value="other">
      Other
    </label>
  </div>

  <div>
    <label for="interests">Interests (Multiple Select):</label>
    <select id="interests" name="interests" multiple>
      <option value="sports">Sports</option>
      <option value="music">Music</option>
      <option value="reading">Reading</option>
      <option value="travel">Travel</option>
      <option value="technology">Technology</option>
    </select>
  </div>

  <div>
    <label for="file">Upload File:</label>
    <input type="file" id="file" name="file" accept=".pdf,.doc,.docx">
  </div>

  <div>
    <label for="disabled-field">Disabled Field:</label>
    <input type="text" id="disabled-field" name="disabled-field" value="This is disabled" disabled>
  </div>

  <div>
    <label for="readonly-field">Read-only Field:</label>
    <input type="text" id="readonly-field" name="readonly-field" value="This is read-only" readonly>
  </div>

  <div>
    <button type="submit">Submit</button>
    <button type="reset">Reset</button>
    <button type="button">Cancel</button>
  </div>

</form>

