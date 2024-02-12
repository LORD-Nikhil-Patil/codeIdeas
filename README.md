# codeIdeas

# cached search input

import React, { useEffect, useState, useDeferredValue } from "react";
import "./styles.css";

export default function App() {
  const [todos, setTodos] = useState([]);
  const [searchQuery, setSearchQuery] = useState("");
  const searchTextDeferred = useDeferredValue(searchQuery);
  const [cache, setCache] = useState({});

  const fetchData = async () => {
    if (cache[searchTextDeferred]) {
      console.log("Fetching from cache");
      setTodos(cache[searchTextDeferred]);
    } else {
      console.log("Fetching from API");
      const getTodos = await fetch(
        `https://dog.ceo/api/breed/${searchTextDeferred}/images`
      );
      const todosTojson = await getTodos.json();
      setCache((prevCache) => ({
        ...prevCache,
        [searchTextDeferred]: todosTojson,
      }));
      setTodos(todosTojson);
    }
  };

  useEffect(() => {
    fetchData();
  }, [searchTextDeferred]);

  console.log(todos);
  return (
    <div className="App">
      <input type="text" onChange={(e) => setSearchQuery(e.target.value)} />
    </div>
  );
}
