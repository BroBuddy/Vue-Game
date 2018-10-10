<template>
    <div class="col-sm-12 col-md-6 col-lg-4">
        <div class="panel panel-info">
            <div class="panel-heading">
                Items at Character
            </div>

            <div class="panel-body">
                <table class="table table-striped table-hover" v-if="character.length >= 1">
                    <thead>
                        <tr>
                            <th>Item</th>
                            <th>Power</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in character"
                        :class="{ warning: (load + item.weight) > maxLoad() }">
                            <td>
                                <img data-toggle="tooltip" data-html="true"
                                :title="getTooltip(item.title, item.level, item.weight, item.price)"
                                :src="getImage(item.type, item.level)" alt="" />
                            </td>
                            <td>{{ item.power }}</td>
                            <td>
                                <div class="btn-group pull-right" role="group" aria-label="...">
                                    <button class="btn btn-sm btn-success"
                                    type="button"
                                    v-if="maxLoad() >= (load + item.weight)"
                                    @click="itemLocation(index, item, 'Backpack', 'Character')">
                                        <span class="glyphicon glyphicon-transfer" aria-hidden="true"></span>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <div v-if="character.length <= 0">
                    Your character is completely naked.
                </div>
            </div>

            <div class="panel-footer" v-if="character.length >= 1">
                {{ character.length }} Item(s)
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['character', 'load', 'maxLoad'],
        methods: {
            itemLocation(index, item, newLocation, oldLocation) {
                this.$emit('addItem', { item, newLocation });
                this.$emit('removeItem', { index, item, oldLocation });
            },
            getImage(type, level) {
                var url = '/assets/' + type + '/' + level + '.png';
                return url;
            },
            getTooltip(title, level, weight, price) {
                return '<p><strong>' + title + '</strong><br/><em>Level ' + level + '</em></p><p>' + weight + 'kg | ' + price + '$</p>';
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>