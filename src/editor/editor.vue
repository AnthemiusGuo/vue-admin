<template>
    <table class="table">
        <tr v-for="row in fields">
            <template v-for="item in row">
                <td>{{item.label}}</td>
                <td :colspan="row.length===1?3:1">
                    <component 
                        :is="item.editor" 
                        v-model="item.value" 
                        :candidate="item.candidate"
                        :placeholder="item.placeholder"
                        :uri="item.uri"
                        :valuefield="item.valuefield"
                        :labelfield="item.labelfield"
                        :relates="item.relates"
                        :key="item.field"
                    ></component>
                </td>
            </template>
        </tr>
    </table>
</template>

<script>
import field_array_model from "./field_array_model"
import field_async_array_model from "./field_async_array_model"
import field_async_enum_radio from "./field_async_enum_radio"
import field_async_enum_select from "./field_async_enum_select"
import field_async_model from "./field_async_model"
import field_async_tag from "./field_async_tag"
import field_bool from "./field_bool"
import field_day from "./field_day"
import field_enum_radio from "./field_enum_radio"
import field_enum_select from "./field_enum_select"
import field_model from "./field_model"
import field_month from "./field_month"
import field_number from "./field_number"
import field_pwd from "./field_pwd"
import field_relates_array_model from "./field_relates_array_model"
import field_relates_enum_radio from "./field_relates_enum_radio"
import field_relates_enum_select from "./field_relates_enum_select"
import field_relates_model from "./field_relates_model"
import field_relates_tag from "./field_relates_tag"
import field_sex from "./field_sex"
import field_string from "./field_string"
import field_tag from "./field_tag"
import field_text from "./field_text"
import field_text_rich from "./field_text_rich"
import field_ts from "./field_ts"
import field_year from "./field_year"



import {observe_relates} from "./field_relates_helper.js"
export default{
    data(){
        return {
            proxyFields:{},
        }
    },
    components:{
        field_array_model,
        field_async_array_model,
        field_async_enum_radio,
        field_async_enum_select,
        field_async_model,
        field_async_tag,
        field_bool,
        field_day,
        field_enum_radio,
        field_enum_select,
        field_model,
        field_month,
        field_number,
        field_pwd,
        field_relates_array_model,
        field_relates_enum_radio,
        field_relates_enum_select,
        field_relates_model,
        field_relates_tag,
        field_sex,
        field_string,
        field_tag,
        field_text,
        field_text_rich,
        field_ts,
        field_year

    },
    computed:{
        formData(){
            return this.fields.reduce((obj,item)=>{
                item.reduce((obj,item)=>{
                    obj[item.field] = item.value;
                    return obj;
                },obj)
                return obj;
            },{});
        }
    },
    watch:{
        fields:{
            immediate:true,
            handler(newFields){
                this.proxyFields = {};

                newFields.forEach((row)=>{
                    row.forEach((item)=>{
                        Object.defineProperty(this.proxyFields,item.field,{
                            get(){
                                return item.value
                            },
                            set(){

                            },
                            enumerable:true,
                            configurable:true,
                        })
                    })
                });

                newFields.forEach((row)=>{
                    row.forEach((item)=>{
                        if(item.relates){
                            observe_relates(item.relates,this.proxyFields)
                        }
                    })
                });
            }
        },
    },
    props:{
        fields:{
            type:Array,
            required:true
        }
    },
}
</script>