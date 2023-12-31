<script setup>
import { toRefs, defineProps, onMounted } from "vue";
// import { defineProps ,onMounted } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import ProjectCard from "../cards/ProjectCard.vue";

const props = defineProps({
  projects: {
    type: Array,
    required: true
  }
})

const { projects } = toRefs(props)

function projectAnimation() {
  gsap.registerPlugin(ScrollTrigger);
  gsap.defaults({
    ease: "none",
    duration: 5,
  });
  const projects = gsap.timeline();
  projects.from(".project", { yPercent: 100, opacity: 0 });
  ScrollTrigger.create({
    animation: projects,
    trigger: "#projectContainer",
    start: "top top",
    end: "bottom",
    scrub: true,
  });
}

onMounted(() => {
  document.onreadystatechange = () => {
    if (document.readyState == "complete") {
      projectAnimation();
      ScrollTrigger.refresh();
    }
  };
});
</script>

<template>
  <section class="portfolioSection">
    <div id="projectContainer" class="portfolioContent">
      <h2 class="portfolioTitle">Some project I've work</h2>
      <p class="description">
        Let me show some of my first projects and how I have worked with
        vuejs and nuxtjs. Some of these projects are created with
        collaboration of amazing people and using incredible tools.
      </p>
    </div>
    <div v-if="projects.length" class="projects-container">
      <ProjectCard
        v-for="(project, index) in projects"
        :key="index"
        :title="project.name"
        :image="project.image"
        :date="project.date"
        :projectId="project.id"
        :position="index + 1"
        class="project"
      />
    </div>
    <div class="projects-link-container">
      <router-link to="/works" class="projects-link">
        Take a look at my work
      </router-link>
    </div>
  </section>
</template>

<style scoped>
.portfolioSection {
  margin: 3rem 0;
  padding: 0 0.5rem;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

.portfolioContent {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  flex-direction: column;
  padding: 2rem 2.25rem;
  width: 100%;
  max-width: 1440px;
}

.portfolioTitle,
.portfolioContent .description {
  font-family: var(--font-family);
  color: var(--primary-color);
  letter-spacing: var(--letter-spacing);
}

.portfolioTitle {
  font-size: 4.5rem;
  margin: 0;
  text-align: center;
  font-weight: 500;
  margin-bottom: 3rem;
}

.portfolioContent description {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.portfolioContent .description {
  font-size: 1rem;
  width: 100%;
  max-width: 21.25rem;
  text-align: left;
  font-weight: 400;
  color: var(--lightbg-color);
}

.projects-container {
  width: 80%;
  margin: 32px 0;
  display: flex;
  flex-direction: column;
  gap: 80px;
}

.projects-link-container {
  width: 100%;
  max-width: 1024px;
  margin: 0 auto;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.projects-link {
  color: var(--primary-color);
  font-family: var(--font-family);
  font-size: 1rem;
  font-weight: 400;
  width: 200px;
  height: 200px;
  text-decoration: none;
  margin-top: -5%;
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.projects-link:hover {
  border: 1px solid var(--primary-color);
  border-radius: 100%;
}

@media only screen and (max-width: 1024px) {
  .portfolioContent {
    padding: 1rem 0;
  }

  .portfolioContent div {
    justify-content: center;
  }
}

@media only screen and (max-width: 768px) {
  .portfolioTitle {
    font-size: 4.5rem;
  }
  .portfolioContent div p {
    font-size: 1rem;
  }
}

@media only screen and (max-width: 680px) {
  .portfolioTitle {
    font-size: 3rem;
  }
  .portfolioContent div p {
    font-size: 0.75rem;
  }
}
</style>
