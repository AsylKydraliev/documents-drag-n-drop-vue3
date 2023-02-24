<template>
  <div>
    <h4>Документы</h4>
    <DocumentItem />
  </div>
</template>
<script>
import DocumentItem from "@/components/DocumentItem.vue";
export default {
  name: 'DocumentsView',
  components: { DocumentItem },
}
</script>