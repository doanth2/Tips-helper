# Tips-helper
tips api
\\--------------------
import axios from "axios";

export const Tips = (props) => {
    const tipsId = props;
    
    axios
        .get(`${String(process.env.NEXT_PUBLIC_API_URL)}` + `XXXXXXX/${tipsId}`)
        .then((res) => {
            console.log(res.data);
            return(
                res.data
            );
        })
        .catch((error)=>{
            console.log(error);
        });
};
\\--------------
filter
^-------------
import { FilterParameter } from './Type'

export const Filter = {
  equal: <T>(comparisonValue:T,value:T) => {
    return (comparisonValue === value)
  },
  greaterOrEqual: <T>(comparisonValue:T,value:T) => {
    return (comparisonValue <= value)
  }, 
  greater: <T>(comparisonValue:T,value:T) => {
    return (comparisonValue < value)
  }, 
  lessOrEqual: <T>(comparisonValue:T,value:T) => {
    return (comparisonValue >= value)
  }, 
  less: <T>(comparisonValue:T,value:T) => {
    return (comparisonValue > value)
  }, 
  // 条件に一致したデータで絞り込んだリストを返します
  execute: <T,W extends {[key:string]:any}>(list:W[],filterList:FilterParameter<T>[]) => {
    // 各項目(column)に含まれる条件にどれかが合致したらtrueを返します
    const filterRule = <T,W extends {[key:string]:any}>(filter:FilterParameter<T>,rowData:W) => {
      return filter.parameter.some((param) => filter.comparisonFormula(param,rowData[filter.column] as T ))
    }

    // 各項目全てtrueならtrueを返します
    const filterResult = (rowData:W) => {
      const res = filterList.reduce((result:boolean,filter) => {
        if(filterRule(filter,rowData) === false){
          result = false
        }
        return result
      },true)
      return res
    }
    return list.filter((row) => filterResult(row))
  }
}
----------------type

export interface FilterParameter<T> {
  key:string,
  column:string,
  parameter:T[],
  comparisonFormula:(value:T,comparisonValue:T) => boolean
};
