<template>
  <div>
    <Tabs :mainTitle="selectedProject != null ? $t('dashboard.title', { project: selectedProject.name }) : ''">
      <div class="links"></div>

      <div class="tab__pane is-active">
        <div v-if="!(inProgressIsRequests && inProgressIsRequests.length > 0)" class="tab__container has-sidebar">
          <Greeter v-if="projects != null && projects.length === 0" :title="$t('dashboard.greeter.title')"
                   :items="[$t('dashboard.greeter.no_projects')]">
          </Greeter>

          <div class="tab_inner" v-if="selectedProject != null">
            <SidebarInfo
                :heading="$t('dashboard.create_a_package.title')"
                :text="$t('dashboard.create_a_package.help')"/>

            <div class="tab__content">
              <CreatePackage :project="selectedProject"
                             :lastPackage="lastPackage"
                             :software="software"/>
            </div>
          </div>
        </div>

        <div v-if="(inProgressIsRequests && inProgressIsRequests.length > 0)" class="tab__container has-sidebar">
          <div class="tab_inner" v-if="selectedProject != null">
            <div class="flash flash--info">
              {{ $t('dashboard.package_in_progress') }}
            </div>
          </div>
        </div>
      </div><!-- /.tab__pane -->
    </Tabs>

    <Tabs v-if="projects != null && projects.length !== 0">
      <div class="grid-container">
        <div class="grid-col grid-col--6">
          <RequestHistory :isRequests="isRequests"/>
        </div>
        <div class="grid-col grid-col--6">
          <LatestPackages :packages="packages" :software="software"/>
        </div>
      </div>
    </Tabs>

    <Tabs v-if="projects != null && projects.length !== 0">
      <div class="grid-container">
        <div class="grid-col grid-col--12">
          <DeployHistory :environmentRequests="deployRequests"
                         :software="software"
          />
        </div>
      </div>
    </Tabs>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'
  import Tabs from '~/components/Layout/Tabs'
  import Greeter from '~/components/Elements/Greeter'
  import CreatePackage from '~/components/Deployer/CreatePackage'
  import SidebarInfo from '~/components/Elements/SidebarInfo'
  import RequestHistory from '~/components/Deployer/RequestHistory'
  import DeployHistory from '~/components/Deployer/DeployHistory'
  import LatestPackages from '~/components/Deployer/LatestPackages'

  export default {
    middleware: 'auth',
    data () {
      return {
        intervals: []
      }
    },
    computed: {
      ...mapGetters({
        projects: 'project/projects',
        selectedProject: 'project/selectedProject',
        deployRequests: 'environmentRequest/dashboardDeployRequests',
        isRequests: 'isRequest/isRequests',
        inProgressIsRequests: 'isRequest/inProgressIsRequests',
        packages: 'package/packages',
        lastPackage: 'package/lastPackage',
        software: 'software/software'
      }),
      name() {
        return this.$t('dashboard.title');
      }
    },
    watch: {
      selectedProject (selectedProject){
        this.fetchDashboardData(selectedProject);
      }
    },
    mounted (){
      if (this.selectedProject) {
        this.fetchDashboardData(this.selectedProject);
      }

      this.intervals.push(setInterval(() => {
        if (this.selectedProject) {
          this.fetchDashboardData(this.selectedProject);
        }
      }, 10000));
    },
    beforeDestroy() {
      if (this.intervals.length == 0) {
        return;
      }
      this.intervals.forEach(function (el) {
        clearInterval(el);
      })
    },
    methods: {
      fetchDashboardData(project){
        this.$store.dispatch('dashboard/getData', {project: project});
      }
    },
    components: {
      Tabs,
      Greeter,
      CreatePackage,
      RequestHistory,
      DeployHistory,
      SidebarInfo,
      LatestPackages
    },
  }
</script>
