---
layout: new-home
permalink: /
title: "An all-in-one IoT kit built for the cloud"
benefits_content:
  - title: "Record-breaking Performance"
    url: https://aka.ms/iot-devkit-purchase
    icon: "icon puzzle"
    content: "Wifi, OLED display, headphone, microphone, sensors like temperature, humidity, motion, pressure, you will love how much time and money you save to build your best IoT project."
    more_content: "Learn more about IoT DevKit hardware spec"
  - title: "Simplify Development"
    icon: "icon vscode"
    content: "Visual Studio Code is lightweight and your goto editor of choice. It is your all-in-one tool from coding the first line to deploying to the cloud."
  - title: "Cloud Empowers Your IoT Solutions"
    icon: "icon cloud"
    content: "Visual Studio Code is lightweight and your goto editor of choice. It is your all-in-one tool from coding the first line to deploying to the cloud."
comments_content:
  - words: "This eliminates the need to have electronics knowledge and the need to do any wiring or soldering in order to get started prototyping IoT solutions."
    photo: assets/images/testimony-chris-pietschmann.jpg
    name: Chris Pietschmann
    title: BuildAzure.com
  - words: "The sample code included scripts taking advantage of Visual Studio Code’s built-in terminal, so I didn’t have to leave my IDE to use the Azure Portal."
    photo: assets/images/testimony-simon-bisson.jpg
    name: Simon Bisson
    title: InfoWorld
  - words: "Getting excited about this awesome new product from @MicrosoftIoT build a #Cloud powered #IoT app in mins!"
    photo: assets/images/testimony-sciencescope.jpg
    name: ScienceScope
projects: "0, 3, 10"
---

{% assign btn_primary_url = "https://aka.ms/iot-devkit-purchase" %}
{% assign btn_outline_url = "/docs/projects/" %}

<div class="banner">
  <p>
    Azure RTOS is now running on IoT DevKit, <a href="https://aka.ms/az3166-azrtos-getstarted" target="_blank">view more</a>.
  </p>
</div>

<div class="header">
  <div class="inner">
    <div class="content">
      <h1>A<span id="typing">n all-in-one</span> IoT kit</h1>
      <h1>built for the cloud</h1>
      <div class="description">All the sensors and parts you love, no soldering needed. Welcome to cloud IoT
        development.</div>
      <div class="button-group">
        <button class="btn primary" onclick="window.open('{{ btn_primary_url }}')"><span>Get a Kit</span></button>
      </div>
      <div class="devkit">
        <input type="checkbox">
        <div class="back"></div>
        <div class="front"></div>
      </div>
      <a class="certified"
        href="https://catalog.azureiotsolutions.com/details?title=MXChip-IoT-DevKit&source=home-page"
        target="_blank"></a>
    </div>
  </div>
</div>

<!-- <div class="notification">
  <div class="inner">
    <span class="badge orange" data-text="What's New"></span>
    8/23: Learn connect the DevKit to Azure IoT Central application within minutes via IoT Plug and Play &emsp; <a href="https://docs.microsoft.com/en-us/azure/iot-central/howto-connect-devkit-pnp">IoT Plug and Play</a>
  </div>
</div> -->

<div class="benefits">
  <div class="inner">
    <h2>Benefits</h2>
    <div class="details">
      {% for item in page.benefits_content %}
        {% if item.url contains 'http' %}
          {% assign domain = '' %}
        {% else %}
          {% assign domain = site.baseurl %}
        {% endif %}
        <div class="item">
          <div class="{{ item.icon }}"></div>
          <h3>{{ item.title }}</h3>
          <div class="content">{{ item.content }}</div>
          <div class="more">
            <a href="{{ domain }}{{ item.url }}">{{ item.more_content }}</a>
          </div>
        </div>
      {% endfor %}
    </div>
  </div>
</div>

<div class="feedbacks">
  <div class="inner">
    <!-- <h2>Testimonial</h2> -->
    <div class="slider-outer">
      <input type="radio" id="feedback1" name="feedback-slider">
      <input type="radio" id="feedback2" name="feedback-slider" checked>
      <input type="radio" id="feedback3" name="feedback-slider">
      <div class="slider">
      {% for item in page.comments_content %}
        <div class="talk-box">
          <div class="words">{{ item.words }}</div>
          <div class="author">
            <img src="{{ item.photo }}">
            <div class="info">
              <div class="name">{{ item.name }}</div>
              <div class="org">{{ item.tittle }}</div>
            </div>
          </div>
        </div>
      {% endfor %}
      </div>
      <div class="arrow">
        <label for="feedback1" class="left"></label>
        <label for="feedback1" class="right"></label>
        <label for="feedback2" class="left"></label>
        <label for="feedback2" class="right"></label>
        <label for="feedback3" class="left"></label>
        <label for="feedback3" class="right"></label>
      </div>
    </div>
  </div>
</div>

<div class="environment">
  <div class="inner">
    <h2>Integrated Development Environment</h2>
    <div class="subtitle">
      Makes it easy to code, build, deploy and debug on Visual Studio Code
    </div>
    <div class="extension">
      <img src="assets/images/pack.png" class="icon">
      <div class="info">
        <div class="title"><strong>Azure IoT Tools</strong></div>
        <div class="description">
          The ultimate collection of extensions for working with Azure IoT in VS Code!
        </div>
        <div class="button">
          <a href="https://aka.ms/azure-iot-tools">
          <button class="btn primary"><span>View more</span></button>
          </a>
        </div>
      </div>
    </div>

<div class="community">
  <div class="inner">
    <!-- <h2>Community</h2> -->
    <div class="list">
      <div class="item">IoT DevKit:<a href="https://microsoft.github.io/azure-iot-developer-kit/docs/faq/">FAQ</a>
      <a href="http://github.com/Microsoft/azure-iot-developer-kit" class="github">GITHUB</a>
      </div>
    </div>
  </div>
</div>

<div class="footer">
  <div class="social">
    FOLLOW: 
    {% if site.author.github %}
      <a href="http://github.com/{{ site.author.github }}" class="github">GITHUB</a>
    {% endif %}
  </div>
  <div class="copyright">
    Copyright &copy; 2019 Microsoft Corp.
  </div>
</div>
