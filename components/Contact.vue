<script setup lang="ts">

</script>

<template>
    <section class="contact">
        <div class="container container--column">
            <div class="contact__title">
                <h2>A project in mind</h2>
                <p>Let's talk about it | お話ししましょう</p>
            </div>
            <div class="contact__torii">
                <svg viewBox="0 0 350 250">
                    <path d="M80 40 L80 235 L105 235 L105 40 Z" class="silhouette-shape"></path>
                    <path d="M245 40 L245 235 L270 235 L270 40 Z" class="silhouette-shape"></path>
                    <path d="M20 30 Q175 40 330 30 L330 50 Q175 60 20 50 Z" class="silhouette-shape"></path>
                    <path d="M30 70 L30 85 L320 85 L320 70 Z" class="silhouette-shape"></path>
                    <path d="M90 130 L90 145 L260 145 L260 130 Z" class="silhouette-shape"></path>
                </svg>
            </div>
            <form class="contact__form">
                <div class="form-group form-group--contact">
                    <label for="name">Your Name</label>
                    <input type="text" id="name" name="name" required>
                </div>
                <div class="form-group form-group--contact">
                    <label for="email">Email Address</label>
                    <input type="text" id="email" name="email" required>
                </div>
                <div class="form-group form-group--contact">
                    <label for="subject">Subject</label>
                    <input type="text" id="subject" name="subject" required>
                </div>
                <div class="form-group form-group--contact">
                    <label for="message">Your Message</label>
                    <textarea id="message" name="message" rows="5"
                        placeholder="Tell me about your project, ideas, or simply say hello..." required></textarea>
                </div>
            </form>
        </div>
    </section>
</template>