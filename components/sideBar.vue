<template>
  <div class="">
    <Menu :items="items" class="mb-8" />
  </div>
</template>

<script setup>
import Menu from '@/components/menu.vue'
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const items = [
  {
    label: 'Membership',
    page: 'Membership',
    permitParam: 'membershipView',
    icon: 'mdi:user',
    content: [
      { text: 'New Registration', to: '/' },
      { text: 'Approved Member', to: '/cms/membership/approved' },
      { text: 'Rejected Member', to: '/cms/membership/rejected' },
      { text: 'Blacklist', to: '/cms/membership/blacklist' },
      { text: 'Report', to: '/cms/membership/report' },
      { text: 'Logs Update Profile', to: '/cms/membership/logs' },
      { text: 'CMS', to: '/cms/membership/cms', isCMS: true },
    ],
  },
  {
    label: 'Training',
    page: 'Training',
    permitParam: 'trainingView',
    icon: 'fa-solid:chalkboard-teacher',
    content: [
      { text: 'All Training', to: '/cms/training/all' },
      { text: 'Rejected', to: '/cms/training/rejected' },
      { text: 'New Registration', to: '/cms/training/registration' },
      { text: 'Dashboard', to: '/cms/training/dashboard' },
    ],
  },
  {
    label: 'Event',
    page: 'Event',
    permitParam: 'eventView',
    icon: 'bi:calendar-event-fill',
    content: [
      { text: 'All Seminar and Event', to: '/cms/event/all' },
      { text: 'New Registration', to: '/cms/event/registration' },
      { text: 'Rejected', to: '/cms/event/rejected' },
      { text: 'Dashboard', to: '/cms/event/dashboard' },
    ],
  },
  {
    label: 'Promotion',
    page: 'Promotion',
    permitParam: 'promotionView',
    icon: 'streamline:discount-percent-badge-solid',
    content: [{ text: 'Promotion', to: '/cms/promotion' }],
  },
  {
    label: 'Payment',
    page: 'Payment',
    permitParam: 'paymentView',
    icon: 'fluent:payment-48-filled',
    content: [{ text: 'Payment', to: '/cms/payment' }],
  },
  {
    label: 'PDPA',
    page: 'PDPA',
    permitParam: 'pdpaView',
    icon: 'fluent:payment-48-filled',
    content: [
      { text: 'All Consent', to: '/cms/pdpa/all' },
      { text: 'Consent Request', to: '/cms/pdpa/request' },
      { text: 'Informed Consent', to: '/cms/pdpa/informed' },
      { text: 'Consent Report', to: '/cms/pdpa/report' },
      { text: 'CMS', to: '/cms/pdpa/cms', isCMS: true },
    ],
  },
  {
    label: 'Director Pool & Search',
    page: 'directorPool',
    permitParam: 'directorView',
    icon: 'mdi:user',
    content: [
      { text: 'Director Pool', to: '/cms/directorPool/list' },
      { text: 'Director Search', to: '/cms/directorPool/search' },
      { text: 'Requested', to: '/cms/directorPool/request' },
      { text: 'Dashboard', to: '/cms/directorPool/dashboard' },
    ],
  },
  {
    label: 'ESG Resource',
    page: 'esg',
    permitParam: 'esgView',
    icon: 'mdi:user',
    content: [
      { text: 'ESG Resource CMS', to: '/cms/esg/cms', isCMS: true },
      { text: 'Report ESG Resources', to: '/cms/esg/report' },
      { text: 'Dashboard ESG Resources', to: '/cms/esg/dashboard' },
    ],
  },
  {
    label: 'About Thai IOD',
    page: 'About',
    permitParam: 'aboutView',
    icon: 'mdi:user',
    content: [
      { text: 'FAQ', to: '/cms/about/faq', isCMS: true },
      { text: 'Contact Us', to: '/cms/about/contact' },
      { text: 'CMS', to: '/cms/about/cms', isCMS: true },
    ],
  },
  {
    label: 'Facilitators',
    page: 'Facilitators',
    permitParam: 'facilitatorView',
    icon: 'mdi:user',
    content: [
      { text: 'All Facilitators', to: '/cms/facilitators/all' },
      { text: 'New Registration', to: '/cms/facilitators/registration' },
      { text: 'Rejected', to: '/cms/facilitators/rejected' },
      { text: 'Activities', to: '/cms/facilitators/activities' },
      { text: 'Dashboard', to: '/cms/facilitators/dashboard' },
      { text: 'Log Update Profile', to: '/cms/facilitators/log' },
      { text: 'CMS', to: '/cms/facilitators/cms', isCMS: true },
    ],
  },
  {
    label: 'CAC',
    page: 'cac',

    permitParam: 'cacView',
    icon: 'mdi:user',
    content: [{ text: 'CMS', to: '/cms/cac', isCMS: true }],
  },
  {
    label: 'Sponsorship & Donation',
    page: 'sponsorship',

    permitParam: 'sponsorAndDonationView',
    icon: 'f7:money-dollar-circle',
    content: [
      { text: 'Sponsorship', to: '/cms/sponsorship/' },
      { text: 'Partnership', to: '/cms/sponsorship/partnership' },
      { text: 'Donation', to: '/cms/sponsorship/donation' },
    ],
  },
  {
    label: 'Email Template',
    page: 'email',
    permitParam: 'generalView',
    icon: 'mdi:email',
    content: [{ text: 'Email', to: '/cms/email' }],
  },
  {
    label: 'User Management',
    page: 'user-management',

    permitParam: 'userManagementView',
    icon: 'mdi:user',
    content: [{ text: 'User Management', to: '/cms/user-management' }],
  },
  {
    label: 'Assessment & Awards',
    page: 'assessment-awards',

    permitParam: 'assessmentAndAwardView',
    icon: 'f7:money-dollar-circle',
    content: [
      { text: 'CMS', to: '/cms/assessment-awards/cms' },
    ],
  },
]

</script>

<style>

</style>