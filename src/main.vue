<template>
<div>
    <kendo-ganttdatasource ref="ganttdatasource1"
                           transport-read-url="https://demos.telerik.com/kendo-ui/service/GanttTasks"
                           transport-read-data-type="jsonp"
                           transport-update-url="https://demos.telerik.com/kendo-ui/service/GanttTasks/update"
                           transport-update-data-type="jsonp"
                           transport-create-url="https://demos.telerik.com/kendo-ui/service/GanttTasks/create"
                           transport-create-data-type="jsonp"
                           transport-destroy-url="https://demos.telerik.com/kendo-ui/service/GanttTasks/destroy"
                           transport-destroy-data-type="jsonp"
                           :transport-parameter-map="parameterMap"
                           schema-model-id="id"
                           :schema-model-fields="fields">
    </kendo-ganttdatasource>

    <kendo-ganttdependencydatasource ref="ganttdependencydatasource1"
                                     transport-read-url="https://demos.telerik.com/kendo-ui/service/GanttDependencies"
                                     transport-read-data-type="jsonp"
                                     transport-update-url="https://demos.telerik.com/kendo-ui/service/GanttDependencies/update"
                                     transport-update-data-type="jsonp"
                                     transport-create-url="https://demos.telerik.com/kendo-ui/service/GanttDependencies/create"
                                     transport-create-data-type="jsonp"
                                     transport-destroy-url="https://demos.telerik.com/kendo-ui/service/GanttDependencies/destroy"
                                     transport-destroy-data-type="jsonp"
                                     :transport-parameter-map="parameterMap"
                                     schema-model-id="id"
                                     :schema-model-fields="dependencyfields">
    </kendo-ganttdependencydatasource>

    <kendo-gantt id="gantt"
                 data-source-ref="ganttdatasource1"
                 dependencies-data-source-ref="ganttdependencydatasource1"
                 :show-work-hours="false"
                 :show-work-days="false"
                 :snap="false"
                 :height="500"
                 :toolbar="tools"
                 :pdf="pdf">
        <kendo-gantt-view :type="'day'"></kendo-gantt-view>
        <kendo-gantt-view :type="'week'" :selected="true"></kendo-gantt-view>
        <kendo-gantt-view :type="'month'"></kendo-gantt-view>
        <kendo-gantt-column :field="'id'" :title="'아이디'" :width="60"></kendo-gantt-column>
        <kendo-gantt-column :field="'title'" :title="'Title'" :editable="true" :sortable="true"></kendo-gantt-column>
        <kendo-gantt-column :field="'start'" :title="'시작'" :format="'{0:MM/dd/yyyy}'" :width="100" :editable="true" :sortable="true"></kendo-gantt-column>
        <kendo-gantt-column :field="'end'" :title="'종료'" :format="'{0:MM/dd/yyyy}'" :width="100" :editable="true" :sortable="true"></kendo-gantt-column>
    </kendo-gantt>
</div>
</template>
<script>
import { GanttDataSource, GanttDependencyDataSource } from '@progress/kendo-datasource-vue-wrapper';
import { Gantt, GanttColumn, GanttView} from '@progress/kendo-gantt-vue-wrapper';
import NanumSquareRoundR from "./NanumSquareRoundR.ttf";  
import kendo from '@progress/kendo-ui'

export default {
    name: 'App',
    components: {
      'kendo-gantt': Gantt,
      'kendo-gantt-view': GanttView,
      'kendo-gantt-column':GanttColumn,
      'kendo-ganttdatasource': GanttDataSource,
      'kendo-ganttdependencydatasource': GanttDependencyDataSource
    },
    data: function() {
        return {
            pdf: {
              fileName: "Kendo UI Gantt Export.pdf",
              proxyURL: "https://demos.telerik.com/kendo-ui/service/export"
            },
            tools: ["append", "pdf"],
            fields: {
                id: { from: 'ID', type: 'number' },
                orderId: { from: 'OrderID', type: 'number', validation: { required: true } },
                parentId: { from: 'ParentID', type: 'number', defaultValue: null, validation: { required: true } },
                start: { from: 'Start', type: 'date' },
                end: { from: 'End', type: 'date' },
                title: { from: 'Title', defaultValue: '', type: 'string' },
                percentComplete: { from: 'PercentComplete', type: 'number' },
                summary: { from: 'Summary', type: 'boolean' },
                expanded: { from: 'Expanded', type: 'boolean', defaultValue: true }
            },
            dependencyfields: {
                id: { from: 'ID', type: 'number' },
                predecessorId: { from: 'PredecessorID', type: 'number' },
                successorId: { from: 'SuccessorID', type: 'number' },
                type: { from: 'Type', type: 'number' }
            }
        }
    },
    methods: {
        parameterMap: function(options, operation) {
           if (operation !== 'read') {
               return {models: kendo.stringify(options.models || [options])}
           }
        }
    },
    created: function() {
      kendo.culture('ko-KR')
    },
}
</script>

<style>

@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;300;400;500;700;900&display=swap");
@import url("https://cdn.jsdelivr.net/gh/moonspam/NanumSquare@1.0/nanumsquare.css");
/*
          Use the DejaVu Sans font for displaying and embedding in the PDF file.
          The standard PDF fonts do not support Unicode characters.
      */
div {
  font-family: "DejaVu Sans",
    "Arial", sans-serif !important;
  font-size: 12px;
}
@font-face {
  font-family: "NanumSquareRound";
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/NanumSquareRound.woff")
    format("woff");
  font-weight: normal;
  font-style: normal;
}
/*
        The example loads the DejaVu Sans from the Kendo UI CDN.
        Other fonts have to be hosted from your application.
        The official site of the Deja Vu Fonts project is
        https://dejavu-fonts.github.io/.
      */
@font-face {
  font-family: "DejaVu Sans";
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/NanumSquareRound.woff")
    format("woff");
}
@font-face {
  font-family: "DejaVu Sans";
  font-weight: bold;
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/NanumSquareRound.woff")
    format("woff");
}
@font-face {
  font-family: "DejaVu Sans";
  font-style: italic;
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/NanumSquareRound.woff")
    format("woff");
}
@font-face {
  font-family: "DejaVu Sans";
  font-weight: bold;
  font-style: italic;
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_two@1.0/NanumSquareRound.woff")
    format("woff");
}
</style>
