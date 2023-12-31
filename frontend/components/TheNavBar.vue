<script setup>
import { ref, computed } from "vue";

const misc = ref({});

const options = [
  {
    label: "Our Company",
    name: "Our Company",
    children: [
      { label: "Overview", name: "Overview" },
      { label: "Values", name: "Values" },
      { label: "Mission", name: "Mission" },
    ],
  },
  {
    label: "Partner with Us",
    name: "Partner with Us",
    children: [
      { label: "Brokers", name: "Brokers" },
      { label: "Lenders", name: "Lenders" },
    ],
  },
  {
    label: "Services",
    name: "Services",
    children: [
      { label: "Commercial Mortgages", name: "Commercial Mortgages" },
      { label: "Equipment Loans", name: "Equipment Loans" },
      { label: "Lines of Credit", name: "Lines of Credit" },
      { label: "Invoice Financing", name: "Invoice Financing" },
      { label: "Startups", name: "Startups" },
      { label: "SBA Loans", name: "SBA Loans" },
      { label: "Merchant Cash Advance", name: "Merchant Cash Advance" },
    ],
  },
  {
    label: "Industries",
    name: "Industries",
    children: [
      { label: "Real Estate", name: "Real Estate" },
      { label: "Construction", name: "Construction" },
      { label: "Healthcare", name: "Healthcare" },
      { label: "Manufacturing", name: "Manufacturing" },
      { label: "Retail", name: "Retail" },
      { label: "Logistics & Trucking", name: "Logistics" },
      { label: "Technology", name: "Technology" },
    ],
  },
  {
    label: "Resources",
    name: "Resources",
    children: [
      { label: "Blog", name: "Blog" },
      { label: "Industry News", name: "Industry News" },
      { label: "eBooks/Whitepapers", name: "eBooks/Whitepapers" },
      { label: "Case Studies", name: "Case Studies" },
    ],
  },
];
const transformToSEOName = (name) => {
  return name
    .toLowerCase()
    .replace(/\s+/g, "-")
    .replace(/[^a-z0-9\-]/g, "");
};

options.forEach((option) => {
  option.name = transformToSEOName(option.name);
  option.children.forEach((child) => {
    child.name = transformToSEOName(child.name);
  });
});

const windowWidth = ref(0);
const activeSubmenu = ref("");
const isActive = ref(false);

const showMobileNav = computed(() => {
  return windowWidth.value < 1024 && isActive.value;
});

const toggleSubmenu = (menuName) => {
  console.log("menuName", menuName);
  console.log("activeSubmenu.value", activeSubmenu.value);
  activeSubmenu.value = activeSubmenu.value === menuName ? "" : menuName;
};

const toggleMenu = () => {
  console.log("toggleMenu");
  isActive.value = !isActive.value;
  console.log("isActive.value : ", isActive.value);
};

onMounted(() => {
  windowWidth.value = window.innerWidth;

  window.addEventListener("resize", () => {
    windowWidth.value = window.innerWidth;
  });
});

onBeforeUnmount(() => {
  // window.removeEventListener("resize");
});
</script>
<template>
  <div class="hero-head">
    <nav class="navbar h-full flex items-center w-full">
      <div class="container h-fit lg:h-auto">
        <div v-show="!showMobileNav" class="navbar-brand mr-2 ml-2">
          <NuxtLink class="navbar-item" to="/">
            <img class="w-auto h-24 scale-[2.8]" src="/images/logo.svg" />
          </NuxtLink>
          <a
            role="button"
            class="navbar-burger"
            aria-label="menu"
            aria-expanded="false"
            @click="toggleMenu"
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

        <div class="grow" />

        <div
          :class="[
            showMobileNav
              ? 'fixed top-0 right-0 bottom-0 left-0 overflow-y-auto'
              : 'relative',
            'h-full',
          ]"
          :style="{ 'z-index': isActive ? 1000 : 10 }"
        >
          <div
            v-if="showMobileNav"
            class="z-20 flex items-center justify-start bg-[#123262] h-[112px] px-[40px]"
          >
            <NuxtLink class="navbar-item" to="/">
              <img class="w-auto h-24 scale-[2.8]" src="/images/logo.svg" />
            </NuxtLink>
            <div class="grow" />
            <button
              class="hover:bg-transparent bg-transparent"
              @click="() => (isActive = false)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="32"
                height="32"
                viewBox="0 0 24 24"
              >
                <path
                  fill="currentColor"
                  d="M19 6.41L17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12z"
                />
              </svg>
            </button>
          </div>

          <div
            id="navbarStandard"
            class="navbar-menu h-full !bg-transparent"
            :class="{ 'is-active': showMobileNav }"
          >
            <div class="navbar-end">
              <div
                v-for="menu in options"
                :key="menu.label"
                class="navbar-item"
                :class="{
                  'has-dropdown is-hoverable': menu.children && menu.children.length > 0,
                }"
              >
                <NuxtLink
                  event=""
                  class="navbar-link"
                  :to="showMobileNav ? '' : '/' + menu.name.toLowerCase()"
                  @click="
                    () =>
                      showMobileNav ? (misc[menu.label] = !misc[menu.label]) : () => {}
                  "
                >
                  {{ menu.label }}
                </NuxtLink>
                <div
                  v-if="
                    (showMobileNav ? !misc[menu.label] : true) &&
                    menu.children &&
                    menu.children.length > 0
                  "
                  class="navbar-dropdown right-0 !-left-48"
                >
                  <NuxtLink
                    v-for="child in menu.children"
                    :key="child.label"
                    :to="'/' + menu.name.toLowerCase() + '/' + child.name.toLowerCase()"
                    class="navbar-item truncate"
                  >
                    {{ child.label }}
                  </NuxtLink>
                </div>
              </div>
            </div>
            <div
              :class="[
                showMobileNav
                  ? 'w-full h-[48px] bg-[#cca23c]'
                  : 'w-auto h-auto bg-transparent',
                'flex items-center sticky right-0',
              ]"
            >
              <a
                :class="[
                  showMobileNav
                    ? 'rounded-none text-lg shadow-2xl'
                    : 'rounded-md bg-[#cca23c] hover:bg-[#b29235] h-fit w-fit text-base',
                  'w-full h-auto px-4 py-2 text-[#18375F]',
                ]"
                target="_blank"
                href="https://www.nationalbusinesscapital.com/apply-now/?ref=2762241"
              >
                <div v-if="!showMobileNav">
                  <p class="font-medium text-sm">Urgent?</p>
                  <p class="text-xs font-bold">Apply Now for Pre-Approval</p>
                </div>
                <span v-else>Urgent? Apply Now for Pre-Approval</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<style scoped>
.navbar {
  background-color: #003366; /* Dark blue, common in finance industries for trustworthiness */
}
.navbar-logo {
  /*  width: 280%; /* Adjust width as per your requirement */
  /* height: 280%; /* Maintain the aspect ratio */
}

.navbar-item h2,
.navbar-link,
.navbar-item {
  color: #ffffff; /* White color for the text for contrast against the dark navbar */
  font-size: 1.125rem;
}

.navbar-burger {
  color: #ffffff; /* White color for the burger menu */
}

.navbar-link:hover,
.navbar-item:hover {
  background-color: #005699; /* Slightly lighter blue for hover effect */
}

.navbar-item.has-dropdown {
  background-color: #123262;
  margin-top: -8px;
}

.navbar-item.has-dropdown:hover .navbar-link,
.navbar-item.has-dropdown.is-active .navbar-link {
  color: #d4a017; /* A touch of gold for active/hovered dropdowns */
  background-color: #005699; /* Slightly lighter blue for hover effect */
}

.navbar-dropdown {
  background-color: #004882; /* Even darker shade for the dropdown to distinguish it from the main bar */
  border: none; /* Remove any default borders */
}

.navbar-dropdown .navbar-item:hover {
  background-color: #0066aa; /* A bit lighter blue for hover effect in dropdowns */
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
.apply-now-button-wrapper {
  margin-left: auto; /* Push it to the right */
  padding: 0 20px; /* Add some padding */
}

.apply-now-button {
  background-color: #d4a017; /* Gold color to make it stand out */
  color: #003366; /* Dark blue text for contrast */
  padding: 10px 20px; /* Padding for the button */
  border-radius: 5px; /* Rounded corners */
  transition: transform 0.3s ease; /* Transition for the hover animation */
}

.apply-now-button:hover {
  background-color: #b89015; /* Slightly darker shade of gold for hover */
  transform: scale(1.05); /* Slightly increase size when hovered */
  text-decoration: none; /* Remove underlines from link on hover */
}
</style>
