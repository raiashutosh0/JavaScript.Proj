// working with sample data 

import React, { useState } from "react";
import * as d3 from "d3";
import moment from "moment";

const SchedulingGraph = ({ data }) => {
  const [selectedDate, setSelectedDate] = useState(null);

  const margin = { top: 20, right: 30, bottom: 30, left: 40 };
  const width = 600 - margin.left - margin.right;
  const height = 400 - margin.top - margin.bottom;

  const svgRef = React.useRef();

  const xScale = d3
    .scaleTime()
    .domain(d3.extent(data, (d) => new Date(d.item_date)))
    .range([margin.left, width - margin.right]);

  const yScale = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.count)])
    .nice()
    .range([height - margin.bottom, margin.top]);

  const line = d3
    .line()
    .x((d) => xScale(new Date(d.item_date)))
    .y((d) => yScale(d.count));

  const onDateClick = (date) => {
    if (selectedDate === date) {
      setSelectedDate(null);
    } else {
      setSelectedDate(date);
    }
  };

  const renderTimeGraph = () => {
    if (!selectedDate) return null;

    const filteredData = data.filter(
      (d) =>
// seduled are mention here 

        moment(d.item_date).format("YYYY-MM-DD") ===
        moment(selectedDate).format("YYYY-MM-DD")
    );

    const timeFormat = d3.timeFormat("%I %p");

    const timeData = [
      {
        time: "9am - 12pm",
        count: filteredData.filter(
          (d) =>

//time formate are mention here 

            moment(d.schedule_time).format("hh:mm A") >= "09:00 AM" &&
            moment(d.schedule_time).format("hh:mm A") < "12:00 PM"
        ).length,
      },
      {
        time: "12pm - 3pm",
        count: filteredData.filter(
          (d) =>
            moment(d.schedule_time).format("hh:mm A") >= "12:00 PM" &&
            moment(d.schedule_time).format("hh:mm A") < "03:00 PM"
        ).length,
      },
      {
        time: "3pm - 6pm",
        count: filteredData.filter(
          (d) =>
            moment(d.schedule_time).format("hh:mm A") >= "03:00 PM" &&
            moment(d.schedule_time).format("hh:mm A") < "06:
